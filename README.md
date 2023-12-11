# Uppgift för Special Code Team

:mega:  @Special Code Team :mega: 
Uppgift för Strukturera Projekt, Observer Pattern, Singelton Pattern (Managers) och Coroutine

 **Förberedelser**: 
  - Klona Flappy Bird-projektet (nu hola-code-team) från GitHub igen och nyttja din branch.
   - Skapa egna prefabs för bruna och gula cirklar.

Del 1: Projektstrukturering

1. **Organisera Projektet**:
   - Skapa en mapp som heter "Game" i ert Unity-projekt för att innehålla alla filer relaterade till spelet.
   - Använd 'Label'-funktionen i Project-vyn för att märka och organisera filer och mappar effektivt.

Del 2: Skapa en Spawning Coroutine

1. **Implementera Spawning Coroutine**:
   - Använd det givna Coroutine-exemplet som grund.
   - Modifiera och utvidga scriptet för att spawn'a bruna och gula cirklar vid slumpmässiga positioner inom spelet.
   - Se till att korutinen kontinuerligt skapar dessa objekt under spelets gång.

```Csharp
IEnumerator SpawnCoroutine (float width, float length, Transform laneTransform)
    {
        WaitForSeconds waitTime = new WaitForSeconds(5f);
        while (true) {
            Vector3 brownSpawnPos = new Vector3 (Random.Range (0, width), Random.Range (0, length), 0);
            Vector3 yellowSpawnPos = new Vector3 (Random.Range (0, width), Random.Range (0, length), 0);
            Instantiate (brownPrefab, brownSpawnPos, Quaternion.identity, laneTransform);
            Instantiate (yellowPrefab, brownSpawnPos, Quaternion.identity, laneTransform);
            yield return waitTime;
        }
    }
```

**Extra (Valfritt)**
* Editor-kontroller: Lägg till funktionalitet i Editorn som låter användare styra parametrar för spawning, såsom intervall och spawnpositioner.

Del3: 

1. **GameManager Singelton Pattern**:
   - Ändra LogicScript till en Singelton och döp om den till GameManager i  Special-code-team projektet.

Del4:

1. **Observer Pattern**:
   - GameManager: Hanterar spelarens hälsa och utlöser en händelse när den ändras.
   - GameEvent (Spelhändelse): Ansvarig för att sända ut händelsen när hälsan ändras.
   - HealthDisplay (Hälsodisplay): Observerar förändringar i hälsan och uppdaterar gränssnittet därefter.

2. **GameManager/Observer Pattern - Hanterar Hälsan**

GameManager kommer att hantera spelarens hälsa och utlösa en händelse när den ändras.

```csharp
using UnityEngine;
using UnityEngine.Events;

public class GameManager : MonoBehaviour
{
    public UnityEvent onHealthChanged;
    private int health = 100;

    public int Health
    {
        get { return health; }
        set
        {
            if (health != value)
            {
                health = value;
                onHealthChanged.Invoke();
            }
        }
    }

    void Start()
    {
        if (onHealthChanged == null)
            onHealthChanged = new UnityEvent();
    }

    void Update()
    {
        // Exempel: Minska hälsan när mellanslagstangenten trycks ner
        if (Input.GetKeyDown(KeyCode.Space))
        {
            Health -= 10;
        }
    }
}```