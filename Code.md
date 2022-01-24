# Escafe

제가 게임을 만들면서 담당했던 부분은 

1. 메인 메뉴 제작
2. 인트로 구현
3. 게임 맵 제작
4. 문 열림 구현
5. 인벤토리 구현
6. 두번째 문 구현

입니다.

맵 제작 총괄 및 메인메뉴 그래픽 부분은 제작이라고 표기했습니다.

- 인트로 부분입니다.
```C# 
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;
using UnityEngine.SceneManagement;

public class Intro : MonoBehaviour
{
    public float LimitTime;
    public Text text_Timer;
    public Text text_Story; // 수정
    public Text text_Skip;


    //변수가 앞 함수가 뒤
    void Start() {
        //text_Story.text = "2008년 xx국에서 생체 실험을 한다는 첩보를 듣고 생체 병기 탈환을 위한 침투 작전 중 사로잡히고 말았다. 방송소리에 정신을 차려보니 실험실은 붕괴 중이다. 이곳을 빠져 나가 구출해주러온 요원들과 접선하라!";
        text_Skip.text = "A = 스토리 스킵";
        Screen.SetResolution(1920, 1080, true);

    }
    
    void Update()
    {
        int room = Random.Range(0, 3);

        LimitTime -= Time.deltaTime;
        text_Timer.text = " " + Mathf.Round(LimitTime);
        if(LimitTime < 0) {
            text_Story.text = "WASD = 이동 / T상호작용";
            text_Timer.text = " ";
            text_Skip.text = "S = 게임 시작";

            if (Input.GetKeyDown(KeyCode.S))
            {
                Debug.Log("press S");
                if (room == 0)
                {
                    SceneManager.LoadScene("Chapter1-1");
                    Debug.Log("1-1");
                }
                else if (room == 1)
                {
                    SceneManager.LoadScene("Chapter1-2");
                    Debug.Log("1-2");
                }
                else
                {
                    SceneManager.LoadScene("Chapter1-3");
                    Debug.Log("1-3");
                }


            }
        }
        if(Input.GetKeyDown(KeyCode.A)){
            Debug.Log("press A");
            LimitTime = 0;
            text_Skip.text = "S = 게임 시작";

        }
   
    }
}
```
유니티에 있는 텍스트 를 if문을 활용해 게임 맵으로 넘어가게 했습니다. 
게임 맵은 한가지만 두면 재미 요소가 떨어질것 같아 세가지 가능성으로 나누었습니다.


