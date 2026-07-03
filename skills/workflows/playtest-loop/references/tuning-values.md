# Common feel tuning values — names, meanings, starting ranges

A catalog of the named tuning values that come up most often in playtest-and-amend rounds.
Names are conventions, not APIs — keep them consistent within a project so feedback maps to
the same variable every round. Ranges are *starting points* for a readable first version
(see the skill's "slow first" pattern), not final values; units assume a typical 2D scale of
roughly 100 px ≈ 1 m and seconds for time. Always re-judge by play, never by the number.

## Ground movement

| Value | Meaning | Start around | Raise when… | Lower when… |
|-------|---------|--------------|-------------|-------------|
| `max_speed` | horizontal speed cap | 200–300 px/s | world feels small, runs feel slow | player overshoots targets |
| `ground_accel` | rate toward max speed | reach max in 0.3–0.5 s | "sluggish", "heavy" | movement feels twitchy |
| `ground_decel` / `friction` | rate toward zero on release | stop in 0.15–0.3 s | player slides past marks | stops feel abrupt |
| `turn_rate` / `turn_accel` | accel when reversing direction | 1.5–2× `ground_accel` | reversals feel muddy | direction flicks feel snappy-cheap |
| `slope_accel_factor` | extra accel from downhill grade | subtle; player barely notices | hills feel flat | speed runs away downhill |
| `landing_speed_retention` | fraction of horizontal speed kept on landing | 0.8–0.9 | "landing kills momentum" | landings never cost anything |

## Jump and air

| Value | Meaning | Start around | Notes |
|-------|---------|--------------|-------|
| `jump_force` / `jump_velocity` | initial upward impulse | clears ~2× character height | cut 10–20% when "floaty" is reported alongside a gravity raise |
| `rise_gravity` | gravity scale while ascending | 1.0 | lower slightly for a floatier apex only if the game wants hang-time |
| `fall_gravity_multiplier` | gravity scale while descending | 1.4–1.8 | the single most effective anti-float value |
| `coyote_time` | grace after walking off a ledge during which jump still fires | 0.08–0.15 s | invisible when right; "I pressed jump!" when missing |
| `jump_buffer` | how long a jump press is remembered before landing | 0.10–0.15 s | fixes "jump ate my input" on landing |
| `apex_control_bonus` | extra air control near the jump apex | optional | adds precision without changing the arc |
| `max_fall_speed` | terminal velocity | 1.5–2× `max_speed` | uncapped falls read as unfair |

## Grinds, rails, and snap-assist

| Value | Meaning | Start around | Notes |
|-------|---------|--------------|-------|
| `snap_distance` | how close to a rail before it captures | generous: 16–32 px | widen while teaching; narrow only if capture feels sticky |
| `snap_angle_tolerance` | approach angle that still snaps | 30–45° | too narrow reads as "grinds are random" |
| `grind_speed_retention` | speed kept when entering the grind | 0.9–1.0 | grinds should rarely feel like a penalty |
| `grind_exit_boost` | small push on a clean exit | 0–10% | reward, not requirement |
| `balance_drift_rate` | how fast a balance meter drifts (if any) | slow enough to correct twice | pure pressure, not a reflex test, at first |

## Failure and recovery

| Value | Meaning | Start around | Notes |
|-------|---------|--------------|-------|
| `bail_angle` | landing slope/impact angle that triggers a bail | lenient: ≥ 35–45° | always print the triggering value in debug |
| `bail_speed_threshold` | minimum speed for a crash vs a stumble | generous | low-speed mistakes should stumble, not crash |
| `recovery_time` | time from bail to control returning | 0.5–1.0 s | long recoveries punish experimentation |
| `checkpoint_spacing` | distance/time between retry points | ≤ 30 s of play | frequent while tuning; thin out later |

## Camera pressure

| Value | Meaning | Start around | Notes |
|-------|---------|--------------|-------|
| `lookahead_distance` | how far the camera leads the motion | 10–20% of screen width | too little hides hazards at speed |
| `lookahead_smoothing` | lag on the lookahead | 0.2–0.4 s to settle | instant lookahead reads as camera jerk |
| `follow_deadzone` | box the player moves in without camera motion | small at speed, larger idle | see `camera-systems` for implementation |
| `speed_zoom_factor` | zoom-out with speed | subtle | strong zoom hides readability you just tuned |

## Chase / stealth pressure

| Value | Meaning | Start around | Notes |
|-------|---------|--------------|-------|
| `pursuer_speed_ratio` | pursuer speed vs player max | 0.85–0.95 | pressure comes from mistakes costing time, not raw speed |
| `rubber_band_strength` | how much the pursuer slows when far ahead/behind | mild | invisible when right; insulting when strong |
| `obstacle_telegraph_time` | how early a hazard is readable before reaction is needed | ≥ 1.0 s at first speed | scale down only with speed increases |
| `detection_time` | seconds in a vision cone before detection | 1.0–2.0 s | instant detection makes stealth trial-and-error |
| `detection_decay` | how fast suspicion drains out of sight | ~2× fill rate | slow decay reads as "it never forgets" |

## Reading a feel report against this table

1. Find the report's verb: *floaty* → jump/air; *sluggish* → ground accel; *random* →
   snap tolerances or missing failure-reason debug; *unfair* → telegraph/pressure values.
2. Amend the one or two values in that row; leave the rest untouched.
3. Surface the amended values in the debug overlay before handing back the build.
