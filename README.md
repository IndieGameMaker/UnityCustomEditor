# Unity Custom Editor

## 사용 에셋

[출처] https://github.com/ciro-unity/UnityRoyale-Public

[사용에셋 다운로드](assets/UnityRoyal.unitypackage)

## 커스텀 에디터

•	유니티 인스펙터 창을 커스텀하는 기능
•	UI를 더 직관적으로 구성 가능
•	개발 중 반복 작업을 줄이고 생산성 향상
•	디자이너와 팀원이 쉽게 데이터 편집 가능
•	툴 제작이나 에디터 확장에 유용하게 활용

## 커스텀 에디터 제작에 사용되는 클래스

- EditorGUILayout
- EditorGUI
- GUILayout
- GUI

## 주요 UI 항목

| 항목                            | 설명                                    |
| ------------------------------- | --------------------------------------- |
| LabelField                      | 텍스트 라벨 표시                        |
| TextField                       | 한 줄 문자열 입력                       |
| TextArea                        | 여러 줄 문자열 입력                     |
| IntField                        | 정수 입력                               |
| FloatField                      | 실수 입력                               |
| Space                           | 여백 삽입                               |
| HelpBox                         | 설명이나 경고 메시지 박스               |
| Button                          | 버튼 생성 및 클릭 이벤트 처리           |
| HorizontalScope / VerticalScope | UI 배치 정렬 용도                       |
| Toggle                          | 체크박스 (bool 값)                      |
| Slider                          | 슬라이더 UI (min~max 값 사이 float)     |
| Vector2Field / Vector3Field     | 벡터 값 입력                            |
| ObjectField                     | 오브젝트(에셋, GameObject 등) 참조 설정 |
| EnumPopup                       | Enum 타입 선택 드롭다운                 |
| Popup                           | 문자열 배열 기반 드롭다운               |
| ColorField                      | 색상 선택                               |
| CurveField                      | 애니메이션 커브 입력                    |
| RectField                       | Rect 영역 입력                          |
| BoundsField                     | Bounds 영역 입력                        |
| Foldout                         | 접이식 섹션 UI                          |