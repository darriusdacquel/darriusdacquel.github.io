---
layout: project
type: project
image: img/unnamed.png
title: "Dream Flip: A 2D Pixel Platformer"
date: 2024
published: true
labels:
  - C#
  - Maya
  - GitHub
  - Unity
  - Photoshop
  - Premiere Pro
  - InDesign
summary: "A challenging 2D platformer developed in an Intro to Game Design class, highlighting a unique gravity-flipping mechanic."
---

<img class="img-fluid" src="../img/dreamFinal.png">

**Project Overview**  
In my Intro to Game Design class, I collaborated with two classmates on a month-long project to conceptualize, design, and develop a 2D pixel platformer game, 'Dream Flip'. This project involved creating an engaging game atmosphere and a unique gameplay mechanic that allows players to flip gravity.

**Team Roles**
- **Coding**: I was responsible for the coding aspects of the game, including the implementation of the gravity-flipping mechanic which significantly impacts gameplay dynamics.
- **Level Design**: Another team member focused on conceptualizing the levels, ensuring that each level challenges and entertains players.
- **Art**: The third member of our team handled all the art, creating visuals that perfectly matched the game's intended atmosphere.

**Development Challenges**  
Debugging a video game brought its own set of unique challenges, which were considerably different and more complex than any other coding tasks I've tackled. It was not only a technically demanding experience but also extremely rewarding and fun, especially as someone who has grown up with a deep passion for video games.

**Concept Evolution and Learning Experience**  
Our initial concept for 'Dream Flip' was ambitious: we envisioned a game where players could switch between two dimensions, each with its own unique challenges. In one dimension, players would interact with visible objects, while in the other, they would have to navigate and dodge unseen beings. The idea was intriguing, but after presenting it to our classmates and teacher, we quickly realized it was too complex to execute within our one-month timeframe. Our teacher emphasized that we were essentially trying to develop two games simultaneously, which would likely result in an incomplete and unfocused final product.

We began by **storyboarding** to map out the game’s narrative and the player’s journey. This helped us visualize the flow of the game and identify key moments where the gravity-flipping mechanic could be most impactful.


- Watch our prototype here: [Dream Flip Prototype](https://youtu.be/GzXWMyHe2vo)

However, after presenting our storyboards and initial concepts in a series of feedback sessions with our classmates and teacher, it became clear that our original idea was too ambitious. These critical feedback sessions were instrumental in shaping the direction of the game. After two rounds of idea pitching and criticism, we realized that our concept needed to be more focused and achievable. We decided to pivot from our original idea to a more streamlined gameplay mechanic—flipping gravity.

With this new direction in mind, we proceeded to create **mock levels** to experiment with different challenges and gameplay dynamics. This allowed us to test how the gravity-flipping mechanic could be integrated into various level designs, ensuring that it was both fun and intuitive for players.

Next, we developed early **prototypes** to test the game mechanics in action. This step was crucial in identifying potential issues early on and iterating on the gameplay until it felt right.

As part of our hands-on experience, we also focused on **asset creation**, crafting all of our game assets from scratch. This included character sprites, background art, and UI elements, all of which needed to align with the game’s atmosphere and theme.

Throughout this process, we continually sought feedback and embraced **iteration** as a way to refine our game. By sharing our game with classmates and receiving constructive criticism, we were able to make continuous improvements. This iterative approach ensured that our final product was both challenging and enjoyable.

Finally, we delved into the psychology of what makes a game fun, learning about concepts like **flow, challenge, and reward**. This knowledge helped us design levels that were not only difficult but also satisfying to complete.

This project taught us the essential steps involved in pitching an idea within the gaming industry and transforming it into a polished final product. It was a journey of learning, adapting, and ultimately creating something we are proud of.


**Play and Watch**
- Watch our game demo here: [YouTube Demo](https://www.youtube.com/watch?v=bRt4Dj-3v-0&ab_channel=Nobyy)
- Play 'Dream Flip' on our Itch.io page: [Play Dream Flip](https://sephye.itch.io/dream-flip)

**Testing Insights**  
During the final playthrough session in our class, where we all had the opportunity to play each other's games, an interesting observation emerged. None of my classmates were able to beat 'Dream Flip', possibly due to its challenging design, which I was more familiar with as the developer. This initially led me to believe that the game might be too difficult. However, when I later shared the game with friends outside of class, they managed to beat it quite easily. This experience underscored the importance of public testing and obtaining feedback from a diverse range of players with varying skill levels. It highlighted how crucial it is to step outside the development bubble to get true insights into a game's accessibility and player experience.

**Sample Code**
Here’s a snippet of the script I wrote for controlling the game's gravity mechanics:

```csharp
public class GravityController : MonoBehaviour
{
    public bool IsGravityUp { get; private set; } = false;
    private Quaternion targetRotation;
    private Transform characterTransform;
    private float gravityFlipCooldown = 1.0f;
    private float lastGravityFlipTime = -2.0f;
    private AudioManager audioManager;
    private PlayerMovement playerMovement;  // Added to access PlayerMovement
    private float groundedBufferTime = 0.5f;  // Time in seconds for the grounded buffer
    private float lastGroundedTime;  // Time when last grounded

    private void Awake()
    {
        audioManager = FindObjectOfType<AudioManager>();
        playerMovement = GetComponent<PlayerMovement>();
    }
    
    private void Start()
    {
        characterTransform = this.transform;
        targetRotation = characterTransform.rotation;
        ResetGravity();
    }

    void Update()
    {
        if (playerMovement.IsGrounded())
        {
            lastGroundedTime = Time.time;  // Update last grounded time
        }

        if (Input.GetKeyDown(KeyCode.F) && Time.time - lastGravityFlipTime >= gravityFlipCooldown && (Time.time - lastGroundedTime <= groundedBufferTime))
        {
            lastGravityFlipTime = Time.time;
            FlipGravity();
        }

        characterTransform.rotation = Quaternion.Slerp(characterTransform.rotation, targetRotation, Time.deltaTime * 5);
    }

    private void FlipGravity()
    {
        IsGravityUp = !IsGravityUp;

        if (IsGravityUp)
        {
            Physics2D.gravity = new Vector2(0, 9.8f);
            targetRotation = Quaternion.Euler(180, 0, 0);
        }
        else
        {
            Physics2D.gravity = new Vector2(0, -9.81f);
            targetRotation = Quaternion.Euler(0, 0, 0);
        }
        audioManager.PlaySFX(audioManager.gravityFlip);
    }

    public void ResetGravity()
    {
        IsGravityUp = false;
        Physics2D.gravity = new Vector2(0, -9.81f);
        targetRotation = Quaternion.Euler(0, 0, 0);
    }
}
```
