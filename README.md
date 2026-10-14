# SkillArena v0.6.0 — Vargas Packet NPC prototype

Paper 1.20.1 / Java 17 / ProtocolLib 5.1.0.

## Setup
1. Install ProtocolLib 5.1.0 compatible with Paper 1.20.1 on the server.
2. Build via GitHub Actions or `mvn clean package`.
3. Put the resulting SkillArena jar in `plugins/` and restart.
4. `/sa boss spawn` starts the existing Vargas boss with a client-side player model.
5. `/sa boss stop` stops the fight and cleans up NPC packets.

## Design
- Hidden zombie remains authoritative for pathing, HP, collision, and skill ray hits.
- ProtocolLib sends player-info, spawn, equipment, teleport, head rotation, arm swing, destroy and remove-player-info packets.
- Each viewer is synchronized independently; join/leave and world changes are handled by the sync loop.
- Optional signed skin textures can be entered in `config.yml` under `vargas.skin`. Without these values, a default skin is used.
- Client-side interactions are intercepted for basic melee testing; attack classification and anti-cheat integration require further hardening.

## Current limitations
- This is a **prototype**. The Packet NPC has not been compiled or tested against a live Paper 1.20.1 server here.
- Client-side player movement currently uses frequent teleport packets, which may look less smooth than natural interpolation.
- Native player attack animations are limited; custom drawing/sheathe animations are deferred.
- No skin texture/signature is shipped.
- The existing boss skill and AI implementations remain prototypes.


## v0.6.1 Maven dependency fix
ProtocolLib 5.1.0 is resolved from Maven Central as `net.dmulloy2:ProtocolLib:5.1.0` with `provided` scope. Do not commit ProtocolLib.jar to this repository. Install ProtocolLib 5.1.0 separately in the server `plugins/` directory.


## v0.6.2
- Fixed Paper 1.20.1 Particle.ENCHANTMENT_TABLE constant.
- Resolved java.util.Vector vs Bukkit Vector import conflict.
- Used ProtocolLib 5.1.0 PLAYER_INFO packet constant.
- GitHub Actions Maven compilation still needs verification.

## v0.6.4
- Fixed missing `org.bukkit.util.RayTraceResult` import (five compiler errors).

### v0.6.4 build fix
- PacketVargasVisual.send: catch RuntimeException instead of checked InvocationTargetException, which the current ProtocolLib API does not declare.
