# Man_don-t_get_angry_events_detection

#  Gameplay State Recognition System  
### *Man, Don’t Get Angry (Chińczyk)*  

the results are available in form of videos under this link:
https://photos.app.goo.gl/2Qp9ZivMsiX7QdXk9   

##  1. Introduction
This project focuses on automatic recognition of gameplay states in the board game Man, Don’t Get Angry (Chińczyk) using classical computer vision techniques.

The system processes video recordings of the game and extracts structured information about the current state, including pawn positions, dice values, player turns, and detected gameplay events.


##  2. Game Description
*Man, Don’t Get Angry* is a board game for 2–4 players where each player controls four pawns.

**Objective:**
Move all pawns from the base, around the board, and into the home area.

**Key rules:**
- Movement is determined by a six-sided die
- Rolling a 6 allows a pawn to leave the base
- Landing on an opponent captures their pawn
- First player to bring all pawns home wins


##  3. In-Game Elements

### Board
- Fixed movement path
- Color-coded bases and home areas

### Pawns
- 4 per player
- Distinct colors (red and blue)
- Circular tokens

### Dice
- Standard black-and-white six-sided die

### Turn Indicator Cards
- Colored cards indicating current player


## 4. Dataset Description

All data was recorded by the authors and divided into three difficulty levels:

###  Easy
- Top-down view  
- Stable lighting  
- No occlusions  

###  Medium
- Light variations  
- Shadows and reflections  

###  Difficult
- Angled camera  
- Hand occlusions  
- Camera shake  
- Dynamic lighting  

 **Total:** 9 videos (3 per difficulty), each 1–5 minutes long  


## 5. Board Detection & Tracking
- HSV color segmentation (white detection)
- Edge detection + contour analysis
- Largest quadrilateral selected as board
- Perspective transformation (homography)

 If detection fails → previous transformation is reused  


##  6. Pawn Detection & Localization
- HSV thresholding for red & blue pawns
- Morphological operations for noise reduction
- Contour filtering:
  - Area
  - Circularity
  - Color properties

 Output:
- Bounding boxes
- Normalized board coordinates

 Pawn classification:
- Base  
- Board  
- Home  


## 🎲 7. Dice Detection & Recognition
- White color segmentation outside board
- Contour filtering + rotated bounding box
- ROI extraction

### Dot Detection:
- Grayscale conversion  
- Adaptive thresholding  
- Morphological filtering  
- Circular contour detection  

### Dice Roll Detection:
- Position stability  
- Value consistency across frames  


##  8. Card Detection & Turn Recognition
- Color segmentation (red/blue)
- Largest region = active player

 Temporal filtering prevents false detections  


##  9. Gameplay State Representation
The system maintains a structured state including:

- Pawns in base (red/blue)
- Pawns on board (red/blue)
- Pawns in home (red/blue)
- Pawn coordinates (relative to board)
- Current player (card color)
- Dice value
- Gameplay score (who is winning)

 State updates only when all pawns are reliably detected  


##  10. Event Detection

Detected by comparing consecutive states:

- 🎲 Dice roll detection  
- 🚀 Pawn leaving base  
- 💥 Pawn capture  
- 🏁 Pawn entering home  
- 🔄 Turn change  

 Events require temporal consistency (multi-frame validation)


##  11. Score & Winner Detection

- More pawns in home → player leads  
- Equal → draw  

Displayed continuously in UI  


##  12. Visualization & UI

Overlay includes:
- Board outline  
- Pawn bounding boxes + labels  
- Dice value  
- Turn indicator  
- Pawn coordinates  
- Score & winner  
- Event notifications  


## 13. Results

| Difficulty | Performance |
|---------- |------------|
|  Easy    | ~99% accuracy |
|  Medium  | 80–90% |
|  Difficult | 70–80% |

### Observations:
- Best performance in stable conditions
- Errors mainly due to:
  - Occlusions
  - Lighting changes
  - Camera movement

##  14. Conclusions

- Classical computer vision methods can effectively analyze board games
- Color segmentation works well but struggles with similar hues
- Perspective correction significantly improves robustness
- Dice detection is the most challenging component

 Overall, the system successfully recognizes gameplay states without using neural networks.

##  Gameplay Events Summary

- 🎲 Dice Roll Detection  
- 🚶 Pawn Movement  
- 💥 Pawn Capture  
- 🏁 Entering Home Path  
- 🚀 Leaving Base (roll = 6)  
- 🏆 Winner Determination  
