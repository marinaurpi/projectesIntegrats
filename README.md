# Projectes Integrats

# EL RESTAURANT

## Client idea

Joc per móbil ambientat en un restaurant italià. Hi haurà un cambrer i el nen serà el cuiner.
L’objecitu del joc es preparar les comandes correctament (segons la recepta) en el temps requerit i en l’ordre que arriben.
Cada recepta consta de: material (aliments, objectes,..), objecitu (que es vol aconseguir), passo a seguir.

La dificultat del joc dependrà de:
* El número de comandes que entrin
* El número de palts de cada comanda
* La dificultat de les receptes

### Objectius
Treballar en cada recepte son: atenció focalitzada, atenció sostinguda, atenció dividida, atenció alternant; memòria de treball, funcions executives (planificació, seqüenciació, inici i acabament, control d’impulsos, adecuació del comportament a les demandes del joc).

### Exercici final
Després d’arribar al final de cada nivell 10 el nen tindà un temps determinat per moure el cos, ballar,
cridar... Aconseguirà aquests punts gracies a que tindrà un responsable aprop seu que supervisarà si compleix aquesta activitat per tal d’obtenir els punts extra. 

Exercicis:
* Saltar 10 vegades
* Córrer fent tres voltes a on estigui sentat
* Fer 10 flexions (nens de 10-18)
* Fer 10 abdominals (nens de 10-18)


## First scene structure ideas

![kitchen sketch food](https://user-images.githubusercontent.com/48218009/111682890-2a503880-8825-11eb-81a8-1c074f28c8b0.png)
![kitchen sketch principal](https://user-images.githubusercontent.com/48218009/111682900-2d4b2900-8825-11eb-87d5-9f27ae1eb7ad.png)

## Necessary assets

### DEVICES
1. Nevera (tancada, oberta)
1. Forn (tancat, tancat amb llum, obert, obert amb llum)
1. Batidora (oberta, tancada)
1. Bol fruita (4-7 tipus)
1. Rebost (armaris o estanteries?)
1. Torradora (apagada, encesa pa torrades, encesa pa llesques?)

### TOOLS
1.	Got
2.	Plat
3.	Ganivet
4.	Paella
5.	Olla
6.	Tallapizzes
7.	Guants forn
8.	Escorridor
9.	Bol gelat
10.	Cullera gelat

### INGREDIENTS
1.	Begudes:
  * Cocacola (ampolla i got)
  * Fanta (ampolla i got)
  * Aigua (ampolla i got)
  * Llet (ampolla o brick i got)
2.	Aliments:
  * Fruita:
    * Taronja (sensera, pelada i tallada)
    * Pinya (sensera, pelada i tallada)
    * Platan (sensera, pelada i tallada)
    * Poma (sensera, pelada i tallada) **NOT SURE?**
    * Maduixes (senseres, tallades) **NOT SURE?**
    * Nabius (sensers) **NOT SURE?**
    * Kiwi (senser, pelat, tallat) **NOT SURE?**
  * Pa:
    * Pa hamburguesa
    * Pa torrades
    * Pa torrades torrat
    * Llesques pa
    * LLesques pa torrat
  * Verdura:
    * Tomaquet (senser, tallat)
    * Ceba (senser, pelat, tallat)
    * Enciam (senser, tallat)
  * Embutit:
    * Pernil (envas, fora)
    * Salami (envas, fora)
    * Bacon (envas, fora, cuinat)
    * Hamburguesa (envas, fora, cuinat)
    * Pollaste (envas, fora, cuinat)
* Pot mantega
* Mermelada: 
  * Maduixa
  * Préssec 
  * Taronja
* Oli


## Main mechanics

### Item creators

Objects where items will be spawned (fridge, shelves).
#### Code
```
private void OnMouseDown()
    {
        if (Input.GetMouseButtonDown(0)) {
            Vector3 mousePos;
            mousePos = Input.mousePosition;
            mousePos = Camera.main.ScreenToWorldPoint(mousePos);

            Instantiate(item, this.transform);
        }
        
    }
}
```
### Item behaviour

The goal is to make the object movable when pressing and holding the mouse button (or the finger in mobile devices). Also to detect when is not being held anymore as well as detecting where is being positionated.

#### Code

Variables
```
public class boxScript : MonoBehaviour
{
    private bool isBeingHeld = false;
    private bool inInventory = false;
    private bool inBin = false;
    private bool inStoves = false;
    private bool inWaiter = false;
    private bool inTable = false;
    
    private Vector3 mousePos = new Vector3();
    
    private GameObject scenePanel;
    private GameObject inventory;
    // Update is called once per frame
```

Essencial to get the gameobject where the item can be placed just after being created.
```
    void Start()
    {
        scenePanel = GameObject.Find("ScenePanel");
        inventory = GameObject.Find("Inventory");

    }
 ```
 
 Detecting when the item is being held and moving it together with the cursor (or finger).
 ```
    void Update()
    {
        if (isBeingHeld == true)
        {
            mousePos = Input.mousePosition;
            mousePos = Camera.main.ScreenToWorldPoint(mousePos);

            this.gameObject.transform.localPosition = new Vector3(mousePos.x, mousePos.y, 0);

        }
    }
    
 ```
 
 Detecting when is started to being held.
 ```
    private void OnMouseDown()
    {
        if (Input.GetMouseButtonDown(0))
        {
            isBeingHeld = true;
            gameObject.transform.SetParent(null);
        }
    }
    
 ```
 
 Detecting when is being dropped and know on which element is being located of the scene.
 ```

    private void OnMouseUp()
    {
        isBeingHeld = false;

        if (inInventory)
        {
            gameObject.GetComponent<SpriteRenderer>().color = new Color(0, 255, 0);
            gameObject.transform.SetParent(inventory.transform);
            print("inv");
        }

        else if (inBin)
        {
            Destroy(gameObject);
            print("puff");
        }

        else if (inStoves){
            gameObject.GetComponent<SpriteRenderer>().color = new Color(255, 0, 0);
            print("stoves");
        }

        else if (inTable){
            gameObject.GetComponent<SpriteRenderer>().color = new Color(0, 255, 255);
            print("table");
        }

        else if (inWaiter){
            Destroy(gameObject);
            print("delireved");
        }

        else{
            gameObject.GetComponent<SpriteRenderer>().color = new Color(0, 0, 255);
            gameObject.transform.SetParent(scenePanel.transform);
        }
    }
    
 ```
 
 Detecting when the item being held is entering and leaving each diferent parts of the scene.
 ```

    private void OnTriggerEnter2D(Collider2D collision)
    {
        if (collision.gameObject.tag == "Inventory")
        {
            inInventory = true;
        }

        if (collision.gameObject.tag == "Bin")
        {
            inBin = true;
        }

        if (collision.gameObject.tag == "Stoves")
        {
            inStoves = true;
        }

        if (collision.gameObject.tag == "Table")
        {
            inTable = true;
        }

        if (collision.gameObject.tag == "Waiter")
        {
            inWaiter = true;
        }
    }

    private void OnTriggerExit2D(Collider2D collision)
    {
        if (collision.gameObject.tag == "Inventory")
        {
            inInventory = false;
        }

        if (collision.gameObject.tag == "Bin")
        {
            inBin = false;
        }

        if (collision.gameObject.tag == "Stoves")
        {
            inStoves = false;
        }

        if (collision.gameObject.tag == "Table")
        {
            inTable = false;
        }

        if (collision.gameObject.tag == "Waiter")
        {
            inWaiter = false;
        }

    }
}
```
