<h1 align="center"> Bloodloop </h1>
<b>Where</b>: <a href="https://www.bloodloop.com/home"> <b>Bloodloop</b> </a> OR  <a href="https://www.youtube.com/watch?v=hSj4-CQ8qsE"> <b>Bloodloop Trailer</b> </a>  <br />
<b>When</b>: 2023-2025 <br />
<b>Genre</b>: TPS Hero Shooter  <br />
<b>Platform</b>: PC <br /><br />
  
<i> A 5v5 tactical Hero-Shooter game, built on Unreal Engine 5, integrated with blockchain.</i> <br /><br />


In this project i contributed more then the 60 % of the whole development on the programming side, I worked on (blueprint & C++) :

- Gameplay mechanics
- User Interface
- Back-end & front-end development
- Tech Design Tools
- Blueprint Implementation
- Advance Implementation of GAS _**(Gameplay Ability System)**_ 


![Title_Login](https://github.com/user-attachments/assets/8af4e79e-33b4-40aa-9e81-8251b9beec81)








<h1 align="center"> Here Some Hero Abilities </h1>  


<details><summary>🗡️ ROGUE</summary>

### Behind You
<p align="center">
  <a href="https://github.com/user-attachments/files/22663146/BLGameplayAbility_BehindYou.txt"> BLGameplayAbility_BehindYou code extract </a><br />
</p>

* **Type**: Attack Ability  
* **Mechanics**:  
  - Uses an asynchronous target actor to scan enemies within `BehindYouMaxRange`.  
  - Teleports the rogue behind the target.  
  - Applies temporary effects to allow a “back teleport” within `BehindYouBackTeleportTimeWindow`.  
* **Blueprints / Cues**:  
  - Spawns preview: `BP_BL_Rogue_PlacingPreview_BackTeleport`.  
  - Triggers teleport gameplay cues.  

---

### Cloaking Device
<p align="center">
  <a href="https://github.com/user-attachments/files/22663198/BLGameplayAbility_Invisibility.txt"> BLGameplayAbility_Invisibility code extract </a><br />
</p>

* **Type**: Stealth Ability  
* **Mechanics**:  
  - Plays invisibility montage on activation.  
  - Applies controlled delay: `GE_SkillActivationDelay`.  
  - Grants speed boost: `GE_SpeedBoostCloakingDevice`.  
  - Starts invisibility effect: `CGE_Skill_Invisibility` (plus a dedicated custom component).  
* **Sync**:  
  - FMOD audio.  
  - Server-side ability ending management.  

---

### Frag Grenade
<p align="center">
  <a href="https://github.com/user-attachments/files/22663212/BLGameplayAbility_FragGranade.txt"> BLGameplayAbility_FragGranade code extract </a><br />
</p>

* **Type**: Manual Targeting Ability  
* **Mechanics**:  
  - Trajectory preview.  
  - Awaits confirmation: `WaitTargetData / WaitInputRelease` async functions.  
  - Spawns real projectile: `BP_BL_Granade`.  

<p align="center">
  <video src="https://github.com/user-attachments/assets/bc636573-9d6c-427b-8fe0-b12206f64d27" width="400" controls></video>
</p>

</details>

---

<details><summary>🚀 SATELLITE</summary>

### Orbital Strike
<p align="center">
  <a href="https://github.com/user-attachments/files/22662968/BLGameplayAbility_OrbitalStrike.txt"> BLGameplayAbility_OrbitalStrike code extract </a><br />
</p>

* **Type**: Two-Phase Ability  
* **Phases**:  
  - Selection: async task with predicted ground sphere trace and preview target actor.  
  - Confirmation: `Wait Input Pressed (Confirm)`.  
  - Activation: scans enemies within a cylinder-shaped radius.  
* **Effects**:  
  - Applies damage effect: `GE_OrbitalStrikeDamage` (based on `GE_BaseDamage`).  
  - Area of effect defined by `OrbitalStrikeRadius`.  

---

### Nursery
<p align="center">
  <a href="https://github.com/user-attachments/files/22663067/BLGameplayAbility_Nursery.txt"> BLGameplayAbility_Nursery code extract </a><br />
</p>

* **Type**: Support / Healing Station (Two-phase)  
* **Mechanics**:  
  - Spawns healing station: `BP_BL_HealingStation`.  
  - Uses dedicated asynchronous trace to check placement.  
  - Heals allies (`GE_Heal`) within `HealingRadius`.  
* **FX**: Looped audio and visuals until destroyed or ended (looping gameplay cue).  

---

### The Great Scan
* **Type**: Scanning Ability / Burst activation  
* **Mechanics**:  
  - Plays scanning montage.  
  - Applies timed gameplay effect with “Spotted” status and integrated GC execution.  
  - Uses gameplay cue: `GameplayCue.Hero.Skill.TheGreatScan`.  
* **Team Effect**: Reveals detected enemies to the entire team.  

<p align="center">
  <video src="https://github.com/user-attachments/assets/1b8cac23-91cf-4fcb-b694-1696b3a99d09" width="400" controls></video>
</p>

</details>

---

<details><summary>🏹 HUNTER</summary>

### Sixth Sense
* **Type**: Detection Ability  
* **Mechanics**:  
  - Spawns attached volume: `BP_SixSenseVolume`.  
  - Applies “Spotted” effect: `GE_Spotted_Local` (only for local client, not visible to enemies/allies).  
  - Handles enter/exit and related gameplay cues.  
* **FX**: Audio and visual support configured in the ability.  

---

### Arrow Storm
* **Type**: Ultimate  
* **Mechanics**:  
  - Controlled by `ArrowStormDuration` and `ArrowStormFireRate`.  
  - Applies gameplay effect via set-by-caller: `GE_ArrowStorm`.  
  - Executes synchronized bow/weapon montages.  
  - Uses tags to commit cooldown and reset ultimate handler.  

---

### Mines
* **Type**: Deployable Trap  
* **Mechanics**:  
  - Manages multiple charges: `MineCharges`.  
  - Placement range control.  
  - Prevents team kills with targeting filter: `BlTargetDataFilter_OnTeam`.  

<p align="center">
  <video src="https://github.com/user-attachments/assets/a05914e1-e70d-45ed-b92c-e22fd9613b39" width="400" controls></video>
</p>

</details>

---

<details><summary>🔮 TUYA</summary>

### Psyonic Throw
* **Type**: Projectile Attack  
* **Mechanics**:  
  - Controlled by tags and `Event.Montage.PsyonicThrow`.  
  - Applies impact + slow: `Effect.PsyonicThrow`, `.SpeedDebuff`.  
  - Uses dedicated collision profiles: `PsyonicOrbFFA`, `TeamA`, `TeamB`.  
* **Team Safety**: Prevents friendly fire.  

---

### Primordial Shield
<p align="center">
  <a href="https://github.com/user-attachments/files/22663288/BLGameplayAbility_PrimordialShield.txt"> BLGameplayAbility_PrimordialShield code extract </a><br />
</p>

* **Type**: Protective Ability  
* **Mechanics**:  
  - Uses tags, input, and gameplay cue: `GameplayCue.Hero.Skill.PrimordialShield`.  
  - Applies a gameplay effect that increases the `Shield` attribute (based on hero attribute set).  
  - Collision channels: `ShieldTeamA/B`.  
* **Effect**: Erects barriers protecting allies by filtering friend/enemy projectiles.  

---

### Transcendence Ray
<p align="center">
  <a href="https://github.com/user-attachments/files/22663301/BLGameplayAbility_TrascendenceRay.txt"> BLGameplayAbility_TranscendenceRay code extract </a><br />
</p>

* **Type**: Channeled Ability  
* **Mechanics**:  
  - Dedicated skill/input tags and its own gameplay cue (looping GCs).  
  - Applies gameplay effects for channeling + slow debuff.  
* **Collision**:  
  - Profile `TranscendenceRay` ensures the beam only affects valid targets.  

<p align="center">
  <video src="https://github.com/user-attachments/assets/d2843428-0f77-489b-bbb6-619f4635263e" width="400" controls></video>
</p>

</details>

---

<details><summary>⚔️ TITUS</summary>

<p align="center">
  <video src="https://github.com/user-attachments/assets/acc904fb-511a-4a26-8ef9-e184dd560f8c" width="400" controls></video>
</p>

</details>

