# 💥 FX and Materials

## Overview

The FX system handles all **visual feedback** for shock symptoms — such as:
- Sweating
- Pale or discolored skin
- Pulse bursts
- Heat wave distortions
- Blood particle suppression or restart

Each FX component is **spawned dynamically** based on the selected diagnosis, and **cleared automatically** when the diagnosis changes. FX can be easily extended, reused, and targeted to specific anatomical regions using sockets.

---

## 🧱 System Architecture

### FX are defined in `FDiagnosisEffect`

Each diagnosis can contain multiple effects:

```cpp
TArray<FDiagnosisEffect> Effects;
```

Each `FDiagnosisEffect` holds:
- `UNiagaraSystem* NiagaraEffect` — The FX to spawn
- `bool bAutoDestroy` — Should it clean up automatically
- `FName TargetSocket` — Where on the mesh it attaches

These effects are added using the diagnosis builder:

```cpp
.AddEffect(NS_SweatFX, true, FName("Forehead"))
```

---

### Niagara FX Spawn Logic

All effects are spawned inside:

```cpp
UCPP_SimulationManager::ChangeDiagnosis()
```

1. Previous Niagara FX actors are destroyed.
2. New FX are retrieved from the `Effects` array of the diagnosis.
3. For each FX:
   - The mesh socket location is calculated.
   - A `UNiagaraComponent` is spawned at that location.
   - The Niagara system is assigned and activated.

Stored components are cached for cleanup:

```cpp
TArray<UNiagaraComponent*> SpawnedFX;
```

---

## 🎨 Custom Materials for Symptoms

Some shock types use **material parameter collections** or **dynamic material instances** to drive visual feedback, such as:
- Skin pallor or flushing
- Temperature shimmer
- Dehydration effects

To use:
1. Create a **Material Parameter Collection** (MPC) in Unreal.
2. Set it via C++ using:
```cpp
UMaterialParameterCollectionInstance* MPC = GetWorld()->GetParameterCollectionInstance(MyCollection);
MPC->SetScalarParameterValue(FName("SkinPaleness"), 0.6f);
```

3. Connect that parameter to your material nodes (e.g. `Base Color`, `Opacity`).

Optional: Blend two skin materials using a parameter curve or diagnosis duration.

---

## 🔧 Example Setup in DiagnosisBuilder

```cpp
DiagnosisBuilder(this)
  .SetName("Distributive Shock")
  .AddEffect(NS_SweatFX, true, FName("Forehead"))
  .AddEffect(NS_PulseFX, true, FName("Wrist"))
  .Build();
```

---

## 🧪 Debugging FX

### Verify Spawned Locations

Use debug helpers to confirm socket positions:

```cpp
DrawDebugSphere(GetWorld(), SkeletalMesh->GetSocketLocation(SocketName), 10.0f, 12, FColor::Blue, false, 5.0f);
```

### Check FX Reference

- Is the `UNiagaraSystem*` asset valid in the builder?
- Has it been deleted or renamed?
- Try logging FX names:

```cpp
UE_LOG(LogTemp, Warning, TEXT("Spawning FX: %s"), *FX.NiagaraEffect->GetName());
```

### Confirm Niagara Activation

- Check that `NiagaraComponent->Activate(true)` is being called
- Set looping duration inside the Niagara System if needed
- Add Niagara debug components to your level (optional)

---

## 📈 Performance Considerations

| Concern                 | Strategy                                                   |
|-------------------------|------------------------------------------------------------|
| Too many FX spawning    | Pool Niagara components instead of respawning each time    |
| FX doesn’t clean up     | Use `bAutoDestroy = true` OR manually call `DestroyComponent()` |
| Material blending slow  | Use shader LODs or pre-baked materials for critical systems |

---

## 🛠 Tips for Adding New Effects

1. Design your Niagara effect in the Content Browser
2. Add it to a diagnosis with:
```cpp
.AddEffect(NS_CustomFX, true, FName("spine_03"))
```
3. Confirm socket exists on the anatomical mesh
4. Use short looping FX unless the diagnosis has `Tick()` logic that modifies it over time

Optional: Wrap logic inside `UShockBehavior::OnEnter()` or `Tick()` to enable time-based parameter changes.

---

## 🧩 Advanced FX Ideas

- **Sweat FX** that becomes heavier based on blood loss
- **Temperature shimmer** that increases with fever
- **Pulsing FX** synchronized with heart rate
- **Color overlays** based on simulated oxygen saturation
- **Localized particle FX** only for left arm/right leg/etc.

Combine these with indicator logic or runtime simulation parameters to create layered effects.

---
