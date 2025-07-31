
# Cyberpunk Life Simulator v6.2.4

A text-based life simulation game set in a dystopian cyberpunk future. Navigate the neon-drenched streets of Night City, make life-altering choices, manage your stats, and see if you can thrive, survive, or even transcend the cycle of existence.

## 🌟 Features

*   **Deep Life Simulation**: Manage core stats like Health, Happiness, Neural Credits, Intelligence, Charisma, Reputation, and Legacy.
*   **Dynamic Life Path**: Start with a random background (e.g., Street Urchin, Corp Kid, Nomad) that influences your entire journey.
*   **Vast Choice System**: A massive, dynamic set of actions and choices that become available based on your age, stats, wealth, and past decisions.
*   **Tech & Augmentations**: Invest in genetic therapies, cybernetic upgrades, neural implants, and advanced AI to enhance your capabilities.
*   **High-Stakes Events**: Encounter random challenges (system hacks, social crises) and miraculous opportunities that can change your fate in an instant.
*   **Future Prediction**: Use your Neural Credits to "read the data stream," generating a radar chart that predicts your future state.
*   **Metaphysical Logic**: Toggle "Metaphysical Mode" (玄学模式) to embrace chaos, increasing randomness and the chance for extreme outcomes.
*   **Multiple Endings**: Your journey can end in numerous ways—from a quiet retirement or a tragic demise to a legendary ascension or even breaking the very code of reality.
*   **Immersive Aesthetics**: Styled with a rich cyberpunk theme, featuring neon text, animated "Matrix" code backgrounds, and a futuristic UI.

## 🎮 How to Play

1.  **Connect Consciousness**: Click the `链接意识` button to start a new life cycle.
2.  **Begin Your Life**: You will be assigned a random starting age and background, which sets your initial stats.
3.  **Make Choices**: In each turn, a set of action buttons will appear. Each choice has requirements, costs, and consequences that affect your stats. Hover over a button to see its tooltip. Disabled buttons indicate you don't meet the requirements or can't afford the cost.
4.  **Advance Time**: Choosing an action progresses time, and a new set of choices will become available. Your health and happiness will naturally decay as you age.
5.  **Survive**: Keep your Health above 0. If it drops, you may face a game over unless you have the means for a last-ditch effort like emergency cryo-sleep or full-body replacement.
6.  **Uncover Secrets**: Certain choices and high stats can unlock rare events and special story paths, including clues about the nature of your reality.
7.  **Reach the End**: The game concludes when you die, retire, or achieve a special ending. Your final score is evaluated based on your life's achievements and legacy.

## 🛠️ Code Structure

The entire game is encapsulated within a single HTML file, utilizing embedded CSS for styling and JavaScript for all game logic.

### HTML Structure (`<body>`)

The HTML is organized into distinct semantic blocks:

*   `<div class="container">`: The main wrapper for the entire game UI.
*   `<h1>...</h1>` & `<div id="title-border">`: The game title, featuring an animated Matrix-style background on its border.
*   `<div class="visual-area">`: Contains the main visual elements:
    *   `<canvas id="gameCanvas">`: A canvas for the main Matrix background effect.
    *   `<div id="predictionChartContainer">`: A container where the Chart.js radar chart for future predictions is rendered.
*   `<div id="message-box">`: The primary display for story text, event descriptions, and game feedback.
*   `<div id="stats-box">`: A grid that displays all the player's current stats.
*   `<div id="button-container">`: The container where all actionable choice buttons are dynamically generated each turn.
*   `<div id="notification-area">`: A fixed-position area at the bottom of the screen for transient pop-up notifications (e.g., "Success!", "Challenge!").

### CSS (Embedded `<style>`)

The styling is designed to create an immersive cyberpunk atmosphere.

*   **CSS Variables**: A `:root` block defines the color palette (neon cyan, lime, pink) and other theme properties, making the look easily configurable.
*   **Futuristic Fonts**: Uses `Poppins` for readability and `Press Start 2P` for the classic retro-game title font, imported from Google Fonts.
*   **Neon Glow Effects**: Achieved using `text-shadow` and `box-shadow` with neon colors.
*   **Animated Backgrounds**: The "Matrix" effect is created by drawing characters on an HTML `canvas` in the background.
*   **Interactive UI**: Buttons and cards have hover and active states, with smooth transitions and animations like a radial light-up effect.
*   **Responsive Design**: Uses `clamp()` and grid layouts (`repeat(auto-fit, minmax(...))`) to ensure the interface adapts to different screen sizes.

### JavaScript (Embedded `<script>`)

This is the engine of the game. Key components include:

#### **Game State Management**

A set of global variables holds the entire state of the game, including player stats (`年龄`, `健康`, `神经币`, etc.), game status (`游戏开始`), and narrative flags (`hasCycleClue`).

#### **Core Game Loop & Flow**

*   `开始游戏()`: Resets all stats, assigns a random background, and kicks off the game.
*   `下一回合()`: The main function for turn progression. It updates the story message via `getDynamicMessage()` and generates a new set of actions via `generateButtons()`.
*   `performAction(actionDetails, actionKey, ...)`: The central action handler. It's called when any choice button is clicked. It applies stat changes, handles special outcomes for specific actions, grants technology, and advances the game turn.
*   `显示游戏结束(信息, ...)`: Finalizes the game, stops all animations, displays a detailed end-game summary with a final score, and presents the "Restart" button.

#### **Key Systems**

*   **`actions` Object**: A massive JavaScript object that acts as a database for every possible action in the game. Each action has properties like:
    *   `label`: The text displayed on the button (can be a function to show dynamic costs).
    *   `cost`: The cost in Neural Credits (can be a function for dynamic calculation).
    *   `statsChange`: An array of stat modifiers.
    *   `req`: A function that returns `true` or `false`, determining if the action is available.
    *   `tt`: Tooltip text.
    *   `fn`: The function to execute, which typically calls `performAction()`.
*   **`generateButtons()` Function**: A complex and crucial function that filters the entire `actions` object each turn. It checks requirements and costs, then uses a weighted probability system to decide which handful of actions to present to the player, ensuring variety and relevance.
*   **`handleRandomEventsAndChallenges()`**: Called each turn to randomly trigger positive "Miracle" events or negative "Challenge" events, adding unpredictability.
*   **`simulatePredictionWorker()` & `generatePredictionChart()`**: These functions handle the "Predict Future" action. They simulate a potential future outcome and use the **Chart.js** library to create a compelling radar chart visualization of that prediction.
*   **`triggerLastChanceSystem()`**: A failsafe system that activates if the `generateButtons()` function can't find any viable actions for the player. It offers a final, dramatic set of choices to avoid getting stuck.

#### **Visuals & Utilities**

*   **Matrix Effect**: `setupMatrix()` and `drawMatrix()` functions are responsible for the animated, falling character effect on the two canvases. A `debounce` function is used to handle window resizing efficiently.
*   **Utility Functions**: Helper functions like `更新状态()` (to update the stats and UI), `显示通知()` (to create pop-up alerts), and `isPrime()` (used for special, logic-based events).

## 🚀 Dependencies

*   [Google Fonts](https://fonts.google.com/): For the 'Poppins' and 'Press Start 2P' typefaces.
*   [Chart.js](https://www.chartjs.org/): For rendering the radar chart in the "Future Prediction" feature.
