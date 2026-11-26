# Crit Happens

[![Crit Happens 1.3.0](https://img.shields.io/badge/Crit%20Happens-1.3.0-435D8C?style=for-the-badge)](https://github.com/MotoMoto1234/crit-happens/releases/tag/v1.3.0)
[![Foundry VTT 13.351](https://img.shields.io/badge/Foundry%20VTT-13.351-287D3C?style=for-the-badge&logo=foundryvirtualtabletop&logoColor=white)](#compatibility)

[![Buy me a coffee on Ko-fi](https://img.shields.io/badge/Buy%20me%20a%20coffee-FF5E5B?style=for-the-badge&logo=kofi&logoColor=white)](https://ko-fi.com/motomoto0295)

> [!IMPORTANT]
> **My release-testing pledge**  
> I test every build in my live Foundry games before releasing it. Hotfixes are the only exception. This pledge applies to all of my Foundry modules.

Crit Happens plays the rolling player’s chosen sounds for a natural 1 or 20 on a d20 and a natural 100 on a d100.

## Compatibility

Works with Foundry VTT v13 and was checked against the 13.351 source. It works across game systems and requires no other modules.

## Installation

Open Foundry's **Add-on Modules → Install Module**, paste the following **Manifest URL**, and click Install:

```text
https://github.com/MotoMoto1234/crit-happens/releases/latest/download/module.json
```

Alternatively, install manually:

1. Extract the release ZIP into your Foundry User Data `Data/modules` directory. The result should be `Data/modules/crit-happens/module.json`.
2. Restart Foundry and enable **Crit Happens** in your world's Manage Modules screen.
3. Each player opens **Configure Settings → Module Settings → Crit Happens**, sets **Natural 1 sound (d20)**, **Natural 20 sound (d20)**, and **Natural 100 sound (d100)**, and saves. Use a path such as `sounds/victory.ogg` or an accessible audio URL. The file must be hosted where Foundry can load it; a Windows drive filepath will not work. File browsing requires the appropriate Foundry permission; a player can paste a path provided by the GM.

## Behavior

- The settings are client scoped: each browser/client can choose its own sound and volume. They do not follow an account between devices, and accounts sharing the same browser storage share these settings.
- Only the rolling player's client initiates playback, broadcasting its configured filepath and volume through the module's socket. Each unmuted client hears that player's sound, including the roller. Spectators do not initiate playback or substitute their own configured sounds. For example, Bob's natural 20 plays Bob's natural 20 sound on both Bob's and Sue's unmuted clients.
- Enable **Mute Crit Happens on this client** in Module Settings to silence this module for yourself. This includes your own rolls and other players' rolls, stops currently playing module sounds, and cancels pending module playback. Your rolls still broadcast your configured sounds to other players. Other Foundry audio is unaffected. The toggle is saved for this browser/client and defaults to off.
- After upgrading to 1.3.0, reload all connected clients so everyone uses the new module-specific audio delivery and mute control.
- Each listener's Interface volume still applies. Sound files must be accessible to all listeners. Non-blind private rolls also broadcast their matching sound to everyone.
- Checks individual active die results, ignoring modifiers to the total. A d20 showing 19 with +1 does not trigger; a d20 showing 20 with -1 does.
- Discarded and rerolled results do not trigger, including a discarded 20 with disadvantage.
- Plays each matching sound category once per chat message. A message containing both a natural 1 and a natural 20 plays both configured sounds; sounds may overlap. Repeated results in the same category do not repeat the sound. Also handles rolls added by the originating client to a newly created message during the session, including a newly matching category.
- Hidden blind results do not trigger. Loading chat history or editing an old message from an earlier session does not trigger.
- Requires structured dice in a ChatMessage. Rolls never posted to chat, plain text/inline results without message roll data, and rolls created on another user's behalf by a different client are not covered.
- Plays when the chat roll arrives; it does not wait for third-party dice animations.
- Each blank filepath disables only that category. A natural 1 means 1 on a d20; a 1 on a d100 or another die does not trigger. Foundry's Interface volume also controls the sound.

## Validation

Core settings, chat hooks, die-result flags, and broadcast audio playback were checked against the supplied Foundry 13.351 source. Automated mocked-hook tests cover trigger selection, client ownership, visibility, updates, playback failures, and multiple clients receiving one broadcast with the roller's sound. Live Foundry/audio testing is still required.

## License

[MIT License](LICENSE).
