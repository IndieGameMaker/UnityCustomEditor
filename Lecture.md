## 커리큘럼

### PlayerStats.cs

```csharp
using UnityEngine;

public class PlayerStats : MonoBehaviour, IDamageable
{
    public int hp = 100;
    public int mp = 100;

    public bool isGodMode = false;
    
    // 아래 코드는 커스텀 에디터 코드 작성 후
    public void TankDamage(int damage)
    {
        if (isGodMode) return;
        hp -= damage;

        if (hp <= 0)
        {
            Debug.Log("Player 사망");
        }
    }
    
    public void InitPlayerData()
    {
        hp = 100;
        mp = 100;
        isGodMode = false;

        Debug.Log("플레이어 데이터 초기화 완료");
    }
}

public interface IDamageable
{
    void TankDamage(int damage);
}
```


### PlayerStatsEditor.cs

```csharp
using UnityEditor;
using UnityEngine;

[CustomEditor(typeof(PlayerStats))]
public class PlayerStatsEditor : Editor
{
    // 인스펙터 뷰에 GUI를 커스텀하기 위한 기본 메소드
    public override void OnInspectorGUI()
    {
        // 대상 객체를 PlayerStats 타입으로 캐스팅
        var playerStats = (PlayerStats)this.target;
        
        // MonoBehaviour 스크립트 필드
        EditorGUILayout.ObjectField("스크립트", MonoScript.FromMonoBehaviour((MonoBehaviour)playerStats), typeof(MonoScript), false);

        EditorGUILayout.Space();
        EditorGUILayout.HelpBox("주인공 캐릭터 스텟", MessageType.Info);
        EditorGUILayout.Space();

        // 일반적인 입력 필드
        // 플레이어 HP 필드
        playerStats.hp = EditorGUILayout.IntField("HP", playerStats.hp);

        // 슬라이드 입력 필드
        // 플레이어 MP 필드
        playerStats.mp = EditorGUILayout.IntSlider("MP", playerStats.mp, 0, 100);

        EditorGUILayout.BeginHorizontal();
        // 무적 모드 토글 버튼
        if (GUILayout.Button(playerStats.isGodMode ? "일반 모드" : "무적 모드"))
        {
            playerStats.isGodMode = !playerStats.isGodMode;
        }

        // 플레이어 데이터 초기화 버튼
        if (GUILayout.Button("데이터 초기화"))
        {
            playerStats.InitPlayerData();
        }
        EditorGUILayout.EndHorizontal();
    }
}
```

## GameManager.cs

```csharp
using System.Collections;
using UnityEngine;

public class GameManager : MonoBehaviour
{
    [SerializeField] private GameObject enemyPrefab;
    public Transform player;

    private void Start()
    {
        // 랜덤 시드 초기화
        Random.InitState(System.DateTime.Now.Millisecond);
        
        player = GameObject.FindGameObjectWithTag("Player").transform;
        SpawnEnemy();
    }

    public void SpawnEnemy()
    {
        for (int i = 0; i < 10; i++)
        {
            
            Vector2 pos2 = Random.insideUnitCircle.normalized * Random.Range(10.0f, 20.0f);
            Vector3 pos = new Vector3(pos2.x, 0, pos2.y);

            if (Application.isPlaying)
            {
                Quaternion rot = Quaternion.LookRotation(player.position - pos);
                var enemy = Instantiate(enemyPrefab, pos, rot);
            }
            else
            {
                var playerTr = GameObject.FindGameObjectWithTag("Player").transform;
                Quaternion rot = Quaternion.LookRotation(playerTr.position - pos);
                
                var enemy = Instantiate(enemyPrefab, pos, rot);
            }
        }
    }
}

```

### GameManagerEditor.cs

```csharp
using UnityEditor;
using UnityEngine;

[CustomEditor(typeof(GameManager))]
public class GameManagerEditor : Editor
{
    public override void OnInspectorGUI()
    {
        GameManager manager = (GameManager)target;
        
        EditorGUILayout.ObjectField("주인공", manager.player, typeof(Transform), true);

        EditorGUILayout.BeginHorizontal();
        if (GUILayout.Button("적 생성"))
        {
            manager.SpawnEnemy();
        }
        
        if (GUILayout.Button("적 제거"))
        {
            var enemies = GameObject.FindGameObjectsWithTag("Enemy");
            foreach (var enemy in enemies)
            {
                DestroyImmediate(enemy);
            }        
        }
        EditorGUILayout.EndHorizontal();
    }
}

```

---

### Item 생성 , 스크립터블오브젝트 생성

#### ItemData.cs

```csharp
using UnityEngine;

// CharacterData 컴포넌트는 캐릭터의 데이터를 저장하는 용도로 사용됩니다.
public enum ItemType
{
    Weapon,
    Armor,
    Consumable,
    QuestItem
}

public class ItemData : ScriptableObject
{
    public string itemName; // 아이템 이름
    public ItemType type; // 아이템 종류
    public int quantity; // 아이템의 개수
    public bool isMultiple; // 아이템이 여러 개일 수 있는지 여부
}
```

```csharp
using UnityEditor;
using UnityEngine;

public class ItemCreator : EditorWindow
{
    private string itemName = "New Item";
    private int quantity = 1;
    private bool isMultiple = false;
    private ItemType itemType = ItemType.Weapon;

    [MenuItem("Tools/Item Creator")]
    private static void Init()
    {
        GetWindow<ItemCreator>("Item Creator");
    }

    private void OnGUI()
    {
        itemName = EditorGUILayout.TextField("아이템 명", itemName);
        itemType = (ItemType)EditorGUILayout.EnumPopup("아이템 종류", itemType);
        quantity = EditorGUILayout.IntField("보유 수량", quantity);
        isMultiple = EditorGUILayout.Toggle("복수 보유 가능", isMultiple);

        if (GUILayout.Button("아이템 생성"))
        {
            ItemData item = ScriptableObject.CreateInstance<ItemData>();
            item.itemName = itemName;
            item.type = itemType;
            item.quantity = quantity;
            item.isMultiple = isMultiple;
            
            AssetDatabase.CreateAsset(item, $"Assets/{itemName}.asset");
            AssetDatabase.SaveAssets();
        }
    }
}
```