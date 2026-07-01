# TakeMeter — Planning Document

## 1. Community Choice
I selected the **r/nba** subreddit. The discourse within this community is highly varied, ranging from deep statistical breakdowns (cap space, tracking data, play-type efficiencies) to hyper-reactive emotional venting during live game threads, making it an ideal space for evaluating natural language classification.

## 2. Label Taxonomy
*   **analysis**: The post or comment makes an objective, structured argument backed by specific metrics (e.g., TS%, net rating, eFG%), salary cap rules, or tactical/film observations.
    *   *Example 1:* "An objective look at the tracking data shows Luka's true shooting percentage (TS%) spikes from 52.1% to 61.3% when sharing the floor with a secondary playmaker."
    *   *Example 2:* "Due to the second apron restrictions, trading matching salary requires sending exactly 100% or less of the incoming contract value."
*   **hot_take**: A bold, aggressive narrative or controversial prediction/ranking stated as a definitive fact without substantive, empirical evidence.
    *   *Example 1:* "Let's be real: Jokic is an absolute empty-stats player who will never lead a team past the second round of the playoffs."
    *   *Example 2:* "Ant is already a better pure scorer than prime D-Wade ever was and it isn't close."
*   **reactionary_vent**: Pure emotional outbursts, all-caps hype, expressions of anger, or frustration tied directly to a live game event or single play.
    *   *Example 1:* "FIRE THE ENTIRE COACHING STAFF TONIGHT! BLOWING A 20 POINT LEAD IN THE 4TH QUARTER IS UNACCEPTABLE!!!"
    *   *Example 2:* "LET'S GOOOOOOO WHAT A DUNK!!!"

## 3. Hard Edge Cases & Decision Rules
*   **The "Stat-Driven Hot Take"**: When a user presents a highly biased narrative but throws in a single surface-level stat to mask it (e.g., "Player X is garbage look at his 2 turnovers tonight"). 
    *   *Decision Rule:* If the statistic is used without holistic context or a structured analytical framework, it is classified as a `hot_take`.
*   **The "Long-Form Vent"**: A user writes multiple paragraphs detailing why they hate the team's current ownership group.
    *   *Decision Rule:* If the core substance relies entirely on subjective emotional frustration rather than cap math or tactical film study, it is categorized as a `reactionary_vent`.

## 4. Data Collection Plan
Data was collected using a custom pay-per-result script targeting r/nba game threads and post-game threads. The final dataset was randomly sampled and scrubbed of deleted/removed tokens to maintain a natural distribution of raw fan interactions.

## 5. Evaluation Metrics
While accuracy serves as an overall health check, it fails to capture critical errors when severe class imbalances occur. I utilize precision, recall, and F1-score to detect whether the model is over-predicting a single dominant class at the expense of minority classes.

## 6. Definition of Success
A successful model must achieve a minimum macro F1-score of **0.70** and out-perform the zero-shot Llama-3.3 baseline model.

## 7. AI Tool Plan
*   **System Prompt Generation**: Gemini was used to help design the interactive Python labeling script to accelerate dataset annotation.
*   **Stress Testing**: An LLM was used to generate synthetic edge cases to refine prompt design for the zero-shot baseline.
