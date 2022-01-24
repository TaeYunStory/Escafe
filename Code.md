# Escafe

```C
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
            LimitTime = 0; // a 누르면 설명 사라지면서 바로 신넘어가는거 아쉬워서 넣었음
            text_Skip.text = "S = 게임 시작";

        }
   
    }
}
```
