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
    public Text text_Story;
    public Text text_Skip;


    void Start() {
        //text_Story.text = "2008년 xx국에서 생체 실험을 한다는 첩보를 듣고 생체 병기 탈환을 위한 침투 작전 중 사로잡히고 말았다. 
        방송소리에 정신을 차려보니 실험실은 붕괴 중이다. 이곳을 빠져 나가 구출해주러온 요원들과 접선하라!";
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

- 문열림 구현 입니다.

```C# 
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class test : MonoBehaviour
{
    AudioSource audiosrcDoor;
    private Animator animator;

    private void Awake()

    {

        animator = GetComponent<Animator>();

    }
    private void Start()
    {
        audiosrcDoor = GetComponent<AudioSource>();
        audiosrcDoor.Stop();
    }



    public void OnCollisionStay(Collision collision) {
        if (collision.gameObject.CompareTag("Player"))
        {
            if (Input.GetKeyDown(KeyCode.T) && GetKey())
            {           
                for (int i = 0; i < Inventory.slots.Length; i++)
                {
                    if (Item.ItemType.Key == Inventory.slots[i].item.itemType)
                    {
                        Inventory.slots[i].SetSlotCount(-1);
                        animator.SetTrigger("DoorOpen");
                        audiosrcDoor.Play();
                    }
                }

            }
        }
    }
    public bool GetKey(){
        
       
        for (int i = 0; i < Inventory.slots.Length; i++)
        {
          
            if (Inventory.slots[i] != null && Item.ItemType.Key == Inventory.slots[i].item.itemType)
            {
                return true;
                
            }
        }
        return false;
    }

   }
```

- 인벤토리 구현입니다.

인터넷에서 구글링하면서 응용 활용해 보았습니다.

인벤토리 최상 부모 입니다.
```C#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Inventory : MonoBehaviour
{

    [SerializeField]
    private GameObject go_InventoryBase; 
    [SerializeField]
    private GameObject go_SlotsParent;  

    public static Slot[] slots;  

    void Start()
    {
        
        slots = go_SlotsParent.GetComponentsInChildren<Slot>();
    }

    public void AcquireItem(Item _item, int _count = 1)
    {
        if (Item.ItemType.PuzzleONE != _item.itemType)
        {
            for (int i = 0; i < slots.Length; i++)
            {
                if (slots[i].item != null)  
                {
                    if (slots[i].item.itemName == _item.itemName)
                    {
                        slots[i].SetSlotCount(_count);
                        return;
                    }
                }
            }
        }
        if (Item.ItemType.PuzzleTWO != _item.itemType)
        {
            for (int i = 0; i < slots.Length; i++)
            {
                if (slots[i].item != null)
                {
                    if (slots[i].item.itemName == _item.itemName)
                    {
                        slots[i].SetSlotCount(_count);
                        return;
                    }
                }
            }
        }
        if (Item.ItemType.PuzzleTHREE != _item.itemType)
        {
            for (int i = 0; i < slots.Length; i++)
            {
                if (slots[i].item != null)
                {
                    if (slots[i].item.itemName == _item.itemName)
                    {
                        slots[i].SetSlotCount(_count);
                        return;
                    }
                }
            }
        }
    
        for (int i = 0; i < slots.Length; i++)
        {
            if (slots[i].item == null)
            {
                slots[i].AddItem(_item, _count);
                return;
            }
        }
    }

 }

```

게임내 인벤토리 내 아이템을 표시하는 슬롯에 해당하는 부분입니다.
```C#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class Slot : MonoBehaviour
{

    
    
    public Item item; 
    public int itemCount; 
    public Image itemImage;

    [SerializeField]
    private Text text_Count;
    [SerializeField]
    private GameObject go_CountImage;



    private void Start()
    {
        
    }

    private void SetColor(float _alpha)
    {
        Color color = itemImage.color;
        color.a = _alpha;
        itemImage.color = color;
    }

    public void AddItem(Item _item, int _count = 1)
    {
        item = _item;
        itemCount = _count;
        itemImage.sprite = item.itemImage;

        if (item.itemType == Item.ItemType.Key)
        {
            go_CountImage.SetActive(true);
            text_Count.text = itemCount.ToString();
            //itemlist.Add(item);
        }
        else
        {
            text_Count.text = "0";
            go_CountImage.SetActive(false);
        }

        SetColor(1);
    }

    public void SetSlotCount(int _count)
    {
        itemCount += _count;
        text_Count.text = itemCount.ToString();

        if (itemCount <= 0)
            ClearSlot();
    }
    
    public void ClearSlot()
    {
        item = null;
        itemCount = 0;
        itemImage.sprite = null;
        SetColor(0);

        text_Count.text = "0";
        go_CountImage.SetActive(false);
    }

}
```

- 카메라 시점에서 화면상 오브젝트 와 상호작용을 하는 부분입니다.
```C#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class ActionController : MonoBehaviour
{
    [SerializeField]
    private float range;  // 아이템 습득이 가능한 최대 거리

    private bool pickupActivated = false;  
    private RaycastHit hitInfo; 

    [SerializeField]
    private LayerMask layerMask; 

    [SerializeField]
    private Text actionText;  
    [SerializeField]
    private Inventory theInventory;

    AudioSource audiosrc3;

    void Start()
    {
        audiosrc3 = GetComponent<AudioSource>();
    }

    void Update()
    {
        CheckItem();
        TryAction();
    }

    private void TryAction()
    {
        if (Input.GetKeyDown(KeyCode.T))
        {
            CheckItem();
            CanPickUp();
        }
    }

    private void CheckItem()
    {
        if (Physics.Raycast(transform.position, transform.forward, out hitInfo, range, layerMask))
        {
            if (hitInfo.transform.tag == "Item")
            {
                ItemInfoAppear();
            }
        }
        else
            ItemInfoDisappear();
    }

    private void ItemInfoAppear()
    {
        pickupActivated = true;
        actionText.gameObject.SetActive(true);
        actionText.text = hitInfo.transform.GetComponent<ItemPickUp>().item.itemName + " 획득 " + "<color=yellow>" + "(T)" + "</color>";
    }

    private void ItemInfoDisappear()
    {
        pickupActivated = false;
        actionText.gameObject.SetActive(false);
    }

    private void CanPickUp()
    {
        if (pickupActivated)
        {
            if (hitInfo.transform != null)
            {
                //Debug.Log(hitInfo.transform.GetComponent<ItemPickUp>().item.itemName + " 획득 했습니다.");
                audiosrc3.Play();
                theInventory.AcquireItem(hitInfo.transform.GetComponent<ItemPickUp>().item);
                Destroy(hitInfo.transform.gameObject);
                ItemInfoDisappear();
            }
        }
    }
}

```

- 두번 째 문 구현입니다.
```C#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Door : MonoBehaviour
{
    public GameObject OnePuzzle;
    public GameObject OnePuzzlesub;


    private Animator animator;

    private void Awake()

    {

        animator = GetComponent<Animator>();

    }


    public void OnCollisionStay(Collision collision)
    {
        if (collision.gameObject.CompareTag("Player"))
        {
            if (Input.GetKeyDown(KeyCode.T) && GetPuzzle())
            {          
                for (int i = 0; i < Inventory.slots.Length; i++)
                {

                    if (Item.ItemType.PuzzleONE == Inventory.slots[i].item.itemType)
                    {
                        OnePuzzle.SetActive(false);
                        OnePuzzlesub.SetActive(true);
                        animator.SetTrigger("PuzzleCheck");
                        Inventory.slots[i].SetSlotCount(-1);
                    }
                }

            }         
        }
    }
    public bool GetPuzzle()
    {
        for (int i = 0; i < Inventory.slots.Length; i++)
        {
            if (Inventory.slots[i] != null && Item.ItemType.PuzzleONE == Inventory.slots[i].item.itemType)
            {
                return true;
            }
        }
        return false;
    }

}
```
