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