# RLAC Algorithm Flow

### How the Model "Thinks" and Decides

This document visualizes the decision-making process of the RLAC model in plain English.

---

## 1. The Visual Flow

```text
       [ START: RAW DATA ]
               |
               v
    +-------------------------+
    |  STEP 1: SIMPLIFY       |
    |  (Random Projection)    |
    +-------------------------+
               |
               |  The data is too complex. 
               |  We view it from many random 
               |  angles (lines) to simplify it.
               v
    +-------------------------+
    |  STEP 2: SCANNING       | <-------+
    |  (The Search Loop)      |         |
    +-------------------------+         |
               |                        |
               |  For every cluster we have,
               |  we look at every random line.
               v                        |
    +-------------------------+         |
    |  STEP 3: SCORING        |         |
    |  (The "Brain")          |         |
    +-------------------------+         |
               |                        |
               |  We draw the density curve.
               |  Does it look like two mountains?
               |  (Depth Ratio / Kurtosis / Dip)
               |                        |
               |  YES -> Give it a High Score.
               |  NO  -> Give it a Low Score.
               v                        |
    +-------------------------+         |
    |  STEP 4: THE DECISION   |         |
    |  (Pick the Winner)      |         |
    +-------------------------+         |
               |                        |
               |  Compare ALL scores found.
               |  Pick the single BEST split
               |  in the entire dataset.
               v                        |
    +-------------------------+         |
    |  STEP 5: THE CUT        |         |
    |  (Split the Data)       |         |
    +-------------------------+         |
               |                        |
               |  Cut the winning cluster into
               |  Part A (Left) and Part B (Right).
               |                        |
               +------------------------+
               |
        (Repeat until done)
               |
               v
      [ FINISH: FINAL CLUSTERS ] 
```
---
### 2. Step-by-Step Explanation
#### 📷 Phase 1: Preparation (The "Camera" Analogy)
* Imagine a 3D object casting a shadow on a wall. The shadow is 2D (simpler), but it still shows the shape of the object.
* Action: The model takes the high-dimensional data and projects it onto many random "lines" (shadows).
* Why: It is much faster and easier to find gaps in a simple line than in a complex hyper-cube.

#### 🔍 Phase 2: The Search (Finding the Gaps)
* The model looks at the data projected onto these lines. It is looking for a Valley.
* The Check: It draws a curve of the data density.
* The Criteria (Depth Ratio):
    1. Are there two peaks (mountains)?
    2. Is there a deep valley between them?
* Score: The deeper the valley relative to the peaks, the higher the score.
> (Note: If you selected a different method like dip or kurtosis, it simply changes the definition of what a "good gap" looks like.)

#### 🏆 Phase 3: The Global Decision (Survival of the Fittest)
* The model doesn't just split the first thing it sees. It is Greedy.
* It looks at Cluster A. It finds a decent split (Score: 50).
* It looks at Cluster B. It finds a perfect split (Score: 95).
* Decision: It ignores Cluster A for now and splits Cluster B because it is the "clearest" cut.

#### ✂️ Phase 4: Execution & Repeat
1. The data is physically divided into two new groups based on the valley found in Phase 3.
2. The model asks: "Do I have enough clusters yet?"
  * ❌ No: Go back to Phase 2.
  * ✅ Yes: Stop and return the results.

