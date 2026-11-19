---
title: "Advanced Weapon Scripts"
description: "Dokumentacja do zaawansowanych skryptów broni dla projektu Hellish Battle"
icon: shield
embeddable: true
---

<!-- GitBook Template Page Elements -->

# 🎮 Advanced Weapon Scripts for Unity

<blockquote>
  <p><strong>Status:</strong> ✅ Ready for Production<br>
  <strong>Last Updated:</strong> Nov 19, 2026<br>
  <strong>Versions:</strong> v1.0 | v1.1 (enhanced)</p>
</blockquote>

---

## 🔧 Implementacja i Użycie

### Kroki Implementacji:

1. **Stwórz nowy skrypt** w Edytorze Unity (kliknij prawym przyciskiem myszy → Create → C# Script → WeaponSystem)
2. **Przenieś skrypt** do swojej scene lub prefaba broni
3. **Ustaw wartości zmiennych** w komponencie WeaponConfig (np. damagePerShot, magazineSize)
4. **Podłącz punkty strzału** (firePoint) do transformacji, w której ma być strzelane

### Używanie Skryptu:

```csharp
// Pobierz referencję do komponentu WeaponSystem
WeaponSystem weapon = GetComponent<WeaponSystem>();

// Wystrzel pocisk
weapon.FireBullet();

// Załaduj magazynek
weapon.Reload();

// Sprawdź ile pocisków zostało
int bulletsLeft = weapon.GetBulletsLeft();
Debug.Log($"Zostało {bulletsLeft} pocisków w magazynku");

// Ustaw nowa liczbę obrażeń
weapon.SetDamage(50f);
```

---

## 🎮 Funkcje Dodatkowe

### System Priorytetów Celowania

Skrypt automatycznie wyznacza cel, wybierając najbliższy obiekt o warstwie "Enemy" (lub dowolnej innej, którą określisz):

```csharp
public GameObject FindNearestTarget(Vector3 sourcePosition) {
    GameObject nearestTarget = null;
    float minDistance = Mathf.Infinity;
    
    foreach (GameObject target in targets) {
        if (!target.activeInHierarchy || !target.GetComponent<EnemyStats>().isAlive) continue;
        
        float distance = Vector3.Distance(sourcePosition, target.transform.position);
        
        if (distance < minDistance) {
            minDistance = distance;
            nearestTarget = target;
        }
    }
    
    return nearestTarget;
}
```

### Multi-Bullet System (Podwójne strzelanie)

Możesz ustawić, aby broń strzelała wieloma pociskami jednocześnie:

```csharp
[System.Serializable]
public class MultishotConfig {
    public int shotsCount = 1;         // Liczba pocisków na jeden strzał
    public float spreadAngle = 0f;     // Kąt rozprysku między pociskami (stopnie)
}
```

### Recoil Animation System

System animacji odrzutu broni:

```csharp
public class RecoilAnimation : MonoBehaviour {
    [SerializeField] private Transform weaponTransform;
    [SerializeField] private Vector3 recoilDirection = new Vector3(0f, 0.5f, 0f);
    [SerializeField] private float recoilDuration = 0.2f;
    
    public void TriggerRecoil(Vector3 direction) {
        // Animuj broń - cofanie się w kierunku przeciwnym do strzału
        weaponTransform.position += (recoilDirection * direction.z);
        
        // Reset po zakończeniu animacji
        Invoke(nameof(ResetWeaponPosition), recoilDuration);
    }
    
    void ResetWeaponPosition() {
        weaponTransform.position = transform.position;  // Powrót do pierwotnej pozycji
    }
}
```

---

## 📝 Best Practices (Zalecenia)

<details>
<summary><strong>Zalecenie 1:</strong> Sprawdź magazynek przed strzałem</summary>

```csharp
if (bulletsInMagazine <= 0) {
    Reload(); // Automatycznie załaduj magazynek
}
```

</details>

<details>
<summary><strong>Zalecenie 2:</strong> Używaj prefabów efektów wizualnych</summary>

> Nie twórz nowych GameObject w czasie działania gry — używaj prefabów! Dzięki temu unikniesz memory leaks!

</details>

<details>
<summary><strong>Zalecenie 3:</strong> Optymalizacja strzałów</summary>

- Nie uruchamiaj logów w czasie produkcji gry
- Używaj `Debug.Log` tylko podczas dewelopowania

</details>

<details>
<summary><strong>Zalecenie 4:</strong> Obsługa edge cases</summary>

Co się stanie, gdy cel umrze w trakcie strzelania? Upewnij się, że skrypt obsługuje ten przypadek.

</details>

---

## 🔍 Rozwiązywanie Problemów

### Problemy Z Strzałem:

| Problem | Rozwiązanie |
|---------|-------------|
| Pociski nie trafiają do celu | Sprawdź czy `firePoint` jest ustawione i czy `bulletData.speed` nie wynosi 0 |
| Broń strzela zbyt szybko | Zwiększ wartość `fireRate` do akceptowalnych wartości (np. 0.3f-0.5f) |

### Problemy Z Obrażeniami:

| Problem | Rozwiązanie |
|---------|-------------|
| Gracze odbierają obrażenia mimo nie trafienia | Upewnij się, że warstwa `damageLayers` zawiera odpowiednie obiekty (nie "Gracz" lub "UI") |

### Problemy Z Reagowaniem:

| Problem | Rozwiązanie |
|---------|-------------|
| Broń nie cofa się podczas strzału | Sprawdź czy `recoilMagnitude` jest ustawione na poprawną wartość i czy transformacja broni istnieje |

---

## 🎯 Podsumowanie

Ten skrypt stanowi kompletny system broni Unity z następującymi cechami:

| Cecha | Opis | Status |
|-------|------|--------|
| ✅ Dynamiczne celowanie | Automatyczne trafienie celów | Implemented |
| ✅ System odnawiania amunicji | Auto-ładowanie między strzałami | Implemented |
| ✅ Efekty wizualne | Krew, wzmignięcie lufy, efekty trafień | Implemented |
| ✅ Recoil system | Animacja odrzutu broni | Implemented |
| ✅ Priorytety celowania | Inteligentne wykrywanie celów | Implemented |

<blockquote>
  <h4>Wersja v1.0</h4>
  <p>Skrypt jest gotowy do integracji z dowolnym projektem Unity i oferuje solidną bazę dla mechaniki walki w grach! 🎮🔫</p>
  
  <h4>Następne kroki:</h4>
  <ol>
    <li>Zaktualizuj skrypt do wersji `WeaponSystem` w projekcie Unity</li>
    <li>Przetestuj działanie broni w scenie</li>
    <li>Dostosuj wartości (damage, fireRate) do Twoich potrzeb</li>
  </ol>
</blockquote>

---

🔧 **Dokumentacja wygenerowana za pomocą GitBook Sync** | 📚 **Hellish Battle Revamp Project**


## 🔧 Implementacja i Użycie

### Kroki Implementacji:

1. **Stwórz nowy skrypt** w Edytorze Unity (kliknij prawym przyciskiem myszy → Create → C# Script → WeaponSystem)
2. **Przenieś skrypt** do swojej scene lub prefaba broni
3. **Ustaw wartości zmiennych** w komponencie WeaponConfig (np. damagePerShot, magazineSize)
4. **Podłącz punkty strzału** (firePoint) do transformacji, w której ma być strzelane

### Używanie Skryptu:

```csharp
// Pobierz referencję do komponentu WeaponSystem
WeaponSystem weapon = GetComponent'\''WeaponSystem'\'';

// Wystrzel pocisk
weapon.FireBullet();

// Załaduj magazynek
weapon.Reload();

// Sprawdź ile pocisków zostało
int bulletsLeft = weapon.GetBulletsLeft();
Debug.Log($\"Zostało {bulletsLeft} pocisków w magazynku\");

// Ustaw nowa liczbę obrażeń
weapon.SetDamage(50f);
```

## 🎮 Funkcje Dodatkowe

### System Priorytetów Celowania

Skrypt automatycznie wyznacza cel, wybierając najbliższy obiekt o warstwie "Enemy" (lub dowolnej innej, którą określisz):

```csharp
public GameObject FindNearestTarget(Vector3 sourcePosition) {
    GameObject nearestTarget = null;
    float minDistance = Mathf.Infinity;
    
    foreach (GameObject target in targets) {
        if (!target.activeInHierarchy || !target.GetComponent'\''EnemyStats'\''.isAlive) continue;
        
        float distance = Vector3.Distance(sourcePosition, target.transform.position);
        
        if (distance \u003c minDistance) {
            minDistance = distance;
            nearestTarget = target;
        }
    }
    
    return nearestTarget;
}
```

### Multi-Bullet System (Podwójne strzelanie)

Możesz ustawić, aby broń strzelała wieloma pociskami jednocześnie:

```csharp
[System.Serializable]
public class MultishotConfig {
    public int shotsCount = 1;         // Liczba pocisków na jeden strzał
    public float spreadAngle = 0f;     # Kąt rozprysku między pociskami (stopnie)
}
```

### Recoil Animation System

System animacji odrzutu broni:

```csharp
public class RecoilAnimation : MonoBehaviour {
    [SerializeField] private Transform weaponTransform;
    [SerializeField] private Vector3 recoilDirection = new Vector3(0f, 0.5f, 0f);
    [SerializeField] private float recoilDuration = 0.2f;
    
    public void TriggerRecoil(Vector3 direction) {
        // Animuj broń - cofanie się w kierunku przeciwnym do strzału
        weaponTransform.position += (recoilDirection * direction.z);
        
        // Reset po zakończeniu animacji
        Invoke(nameof(ResetWeaponPosition), recoilDuration);
    }
    
    void ResetWeaponPosition() {
        weaponTransform.position = transform.position;  # Powrót do pierwotnej pozycji
    }
}
```

## 📝 Best Practices (Zalecenia)

1. **Zawsze sprawdzaj, czy magazynek nie jest pusty** przed strzałem:
   ```csharp
   if (bulletsInMagazine \u003c= 0) {
       Reload(); // Automatycznie załaduj magazynek
   }
   ```

2. **Używaj prefabów efektów wizualnych** zamiast tworzyć nowe GameObject w czasie działania gry. Dzięki temu unikniesz memory leaks!

3. **Optymalizacja strzałów**: Nie uruchamiaj logów w czasie produkcji gry - używaj Debug.Log tylko podczas dewelopowania.

4. **Obsługa edge cases**: Co się stanie, gdy cel umrze w trakcie strzelania? Upewnij się, że skrypt obsługuje ten przypadek.

## 🔍 Rozwiązywanie Problemów

### Problemy Z Strzałem:
- **Problem**: Pociski nie trafiają do celu.
  - **Rozwiązanie**: Sprawdź czy firePoint jest ustawione i czy bulletData.speed nie wynosi 0.

- **Problem**: Broń strzela zbyt szybko, powodując błędy wydajności.
  - **Rozwiązanie**: Zwiększ wartość fireRate do akceptowalnych wartości (np. 0.3f-0.5f).

### Problemy Z Obrażeniami:
- **Problem**: Gracze odbierają obrażenia, nawet gdy nie zostali trafieni.
  - **Rozwiązanie**: Upewnij się, że warstwa damageLayers zawiera odpowiednie obiekty (nie "Gracz" lub "UI").

### Problemy Z Reagowaniem:
- **Problem**: Broń nie cofa się podczas strzału.
  - **Rozwiązanie**: Sprawdź czy recoilMagnitude jest ustawione na poprawną wartość i czy transformacja broni istnieje.

## 🎯 Podsumowanie

Ten skrypt stanowi kompletny system broni Unity z następującymi cechami:

| Cecha | Opis |
|-------|------|
| ✅ Dynamiczne celowanie | Automatyczne trafienie celów |
| ✅ System odnawiania amunicji | Auto-ładowanie między strzałami |
| ✅ Efekty wizualne | Krew, wzmignięcie lufy, efekty trafień |
| ✅ Recoil system | Animacja odrzutu broni |
| ✅ Priorytety celowania | Inteligentne wykrywanie celów |

Skrypt jest gotowy do integracji z dowolnym projektem Unity i oferuje solidną bazę dla mechaniki walki w grach! 🎮🔫


