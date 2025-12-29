# How It Works: A Simple Guide to the Self-Driving Car Code

This guide explains how the Python code actually makes the car drive itself, broken down into simple parts.

## 1. The Body (The Car)
Just like a real car, our virtual car has physical properties.
- **Position & Movement**: The code tracks where the car is `(x, y)` and which way it's facing.
- **Actions**: It can do 5 things:
    1.  Go Straight
    2.  Turn Left
    3.  Turn Right
    4.  Sharp Left
    5.  Sharp Right

## 2. The Eyes (The Sensors)
The car can't "see" the whole map like we do. It uses 7 mock "sensors" that beam out in front of it (like flashlight beams).
- **What they see**: Each sensor checks a specific spot ahead.
- **The Signal**: If a sensor hits a dark pixel (road), it sends a "1". If it hits a light pixel (sand/wall), it sends a "0".
- **The Result**: The car knows "Okay, there is road to my left, but a wall straight ahead."

## 3. The Brain (The Neural Network)
This is the smartest part. We use a **Deep Q-Network (DQN)**, which is just a fancy name for a "Practice Makes Perfect" machine.

### Input (What goes in)
The brain receives:
*   Readings from the 7 sensors.
*   The direction to the target (Eiffel Tower).
*   The distance to the target.

### Processing (Thinking)
The information passes through layers of "neurons" (math equations). We recently added an extra layer to make it think deeper! It crunches these numbers to decide which of the 5 actions is best right now.

### Output (The Decision)
The brain spits out a score for each action. The car picks the action with the highest score.

## 4. The Teacher (Reinforcement Learning)
The car starts out completely dumb, crashing into everything. It learns through a system of **Rewards and Punishments**:

*   **Bad Dog! (Punishment)**: If it drives onto the sand (outside white area), it gets a huge penalty (-100 points) and the "game" restarts.
*   **Good Dog! (Reward)**: If it gets closer to the target, it gets a small treat (positive points).
*   **Jackpot!**: If it reaches the target, it gets a massive reward (+100 points).

### The "Memory"
The car remembers its past mistakes and successes. It keeps a "diary" (Replay Memory) of:
*"I was here -> I turned left -> I crashed -> That was bad."*
Or
*"I was here -> I went straight -> I got closer -> That was good."*

Periodically, the brain reviews this diary to update its strategy, making it smarter over time.

## 5. The Map (The World)
The map is just an image (`city_map_paris.png`).
- **Black/Dark Grey pixels**: Obstacle (Crash)
- **White/Light pixels**: Road (Safe)

The code simply checks the color of the pixel under the car to know if it's safe or if it just crashed.

## 6. Why We Changed the Settings (Tuning)

### Physics (How the Car Moves)
*   **Sensor Distance (160)**: Increased so the car can see obstacles further away, giving it more time to react.
*   **Sensor Angle (45)**: Widened the field of view so the car doesn't get blind-sided by obstacles on the side.
*   **Speed (4)**: Slowed down the car so the AI has enough processing frames to make decisions before crashing.
*   **Turn Speed (3) & Sharp Turn (12)**: Adjusted to make the car more agile, allowing it to navigate tight corners without getting stuck.

### Hyperparameters (How the Brain Learns)
*   **Batch Size (64)**: Increased the number of memories it reviews at once to make learning more stable and less jittery.
*   **Gamma (0.99)**: Set high so the car cares about long-term goals (reaching the target) rather than just immediate survival.
*   **Learning Rate (0.0005)**: Lowered to prevent the AI from drastically changing its mind too quickly, which ruins what it has already learned.
*   **Tau (0.005)**: Reduced to ensure the "Target Network" updates slowly, keeping the learning targets stable.
