# HW 4: *3D Stylization* Submission (Charles Wang)

## 1. Concept art

All three pieces are by [OMOCAT](https://x.com/_omocat) and her team!

| Piece 1 | Piece 2 | Piece 3 |
|-------|------|-------|
|![](Writeup/1.png)|![](Writeup/2.png)|![](Writeup/3.png)|

These static images don't do the art style justice. Apart from the colored pencil look, I specifically want to recreate the art style for OMORI, which is known to have animated outlines around its characters.

## 2. Interesting Shaders

I added multiple light support by following the tutorial.

### Specular highlight

I didn't like how the specular highlight looked in my scene so I don't use it in the final setup, but here I show the implementation working (and it's available as a separate Shader Graph called "ToonWithSpecular" in the Shaders folder).

https://github.com/user-attachments/assets/e8e63a89-4431-4245-ad82-92d30d25df51

### Interesting shadow

To emulate the scribble-y look of OMORI, I used the pencil brush from Procreate to create this 512x512 texture, and tiled it using the website:

![](Assets/Textures/shadow.png)

### Special surface shader

To create the "selected" shader, I drew inspiration from two sources in the game. The first is how sometimes the art likes to invert colors. See [this YouTube video](https://www.youtube.com/watch?v=dU_Wbl5va3E) and look at the bottom left corner, OMORI flips colors. Also this picture:

![](Writeup/opp.jpg)

I also wished to animate the selected object using the drawn look as seen in the game, which I did by reusing the shadow texture and overlaying it on the object. I used time, a triangle wave, and floored it so I could get staggered values to use to rotate the UV coords, allowing me to "animate" it. Here's a pic of the Graphtoy I used to plan it:

![](Writeup/graph.png)

For this shader, I also went back to the shadow and made it animated in the same manner, using a different offset/time scale so it looks different from the actual object.

## 3. Outlines

I first implemented basic outlines using the Roberts Cross method.

![](Writeup/basic-outlines.png)

However, outlines in OMORI art seem to be made up of three parts: a green, pink, and purple piece all overlayed on each other.

![](Writeup/outlineex.png)

So I do just that in my shader, blending between them using Darken. In addition, each of the three outlines are customized by slightly offsetting the UVs to give a more "chromatic aberration" look, and I use the Random Range node to vary the thickness of the outlines depending on screen position. Finally, I use the same animation technique that I used in the surface shader above. Final result:

![](Writeup/cool-outlines.png)

## 4. Full Screen Post Process Effect

Inspired by the white border that surrounds the first concept art piece, I decided to implement a white-colored vignette that goes around the screen borders.

## 5. Create a Scene

Models and credits:

- "Picnic Basket from Poly by Google" (https://skfb.ly/6XXuR) by IronEqual is licensed under Creative Commons Attribution (http://creativecommons.org/licenses/by/4.0/).
- "Low Poly Laptop" (https://skfb.ly/o9G6r) by zgoosr is licensed under Creative Commons Attribution (http://creativecommons.org/licenses/by/4.0/).
- "Low Poly Picnic Asset Pack" (https://skfb.ly/6TtZR) by lizhiqiang is licensed under Creative Commons Attribution (http://creativecommons.org/licenses/by/4.0/).
- "Door" (https://skfb.ly/opVZ9) by Arnau Rocher Alcayde is licensed under Creative Commons Attribution (http://creativecommons.org/licenses/by/4.0/).

For the laptop, I created a custom shader that specifically draws a texture to the screen geometry. To prevent the door from self-shadowing (and looking uglier), I added a `UseShadow` parameter that completely turns off shadow attenuation in the main toon shader. (We're not exactly being realistic in this HW.)

## 6. Interactivity
As a finishing touch, let's show off the fact that our scene is rendered in real-time! Please add an element of interactivity to your scene. Change some major visual aspect of your scene on a keypress. The triggered change could be
* Party mode (things speed up, different colorization)
* Memory mode (different post-processing effects to color you scene differently)
* Fanart mode (different surface shaders, as if done by a different artist)
* Whatever else you can think of! Combine these ideas, or come up with something new. Just note, your interactive change should be at least as complex as implementing a new type of post processing effect or surface shader. We'll be disappointed if its just a parameter change. There should be significant visual change.

### To-Do:
* Create at least one new material to be swapped in using a key press
* Create and attach a new C# script that listens for a key press and swaps out the material on that key press. 
Your C# script should look something like this:
```
public Material[] materials;
private MeshRenderer meshRenderer;
int index;

void Start () {
          meshRenderer = GetComponent<MeshRenderer>();
}

void Update () {
          if (Input.GetKeyDown(KeyCode.Space)){
                 index = (index + 1) % materials.Count;
                 SwapToNextMaterial(index);
          }
}

void SwapToNextMaterial (int index) {
          meshRenderer.material = materials[index % materials.Count];
}
```
* Attach the c# script as a component to the object(s) that you want to change on keypress
* Assign all the relevant materials to the Materials list field so you object knows what to swap between.
 
---
## 7. Extra Credit
Explore! What else can you do to polish your scene?
  
- Implement Texture Support for your Toon Surface Shader with Appealing Procedural Coloring.
    - I.e. The procedural coloring needs to be more than just multiplying by 0.6 or 1.5 to decrease/increase the value. Consider more deeply the relationship between things such as value and saturation in artist-crafted color palettes? 
- Add an interesting terrain with grass and/or other interesting features
- Implement a Custom Skybox alongside a day-night cycle lighting script that changes the main directional light's colors and direction over time.
- Add water puddles with screenspace reflections!
- Any other similar level of extra spice to your scene : ) (Evaluated on a case-by-case basis by TAs/Rachel/Adam)

## Submission
1. Video of a turnaround of your scene
2. A comprehensive readme doc that outlines all of the different components you accomplished throughout the homework. 
3. All your source files, submitted as a PR against this repository.

## Resources:

1. Link to all my videos:
    - [Playlist link](https://www.youtube.com/playlist?list=PLEScZZttnDck7Mm_mnlHmLMfR3Q83xIGp)
2. [Lab Video](https://youtu.be/jc5MLgzJong?si=JycYxROACJk8KpM4)
3. Very Helpful Creators/Videos from the internet
    - [Cyanilux](https://www.cyanilux.com/)
        - [Article on Depth in Unity | How depth buffers work!](https://www.cyanilux.com/tutorials/depth/) 
    - [NedMakesGames](https://www.youtube.com/@NedMakesGames)
        - [Toon Shader Lighting Tutorial](https://www.youtube.com/watch?v=GQyCPaThQnA&ab_channel=NedMakesGames)
        - [Tutorial on Depth Buffer Sobel Edge Detection Outlines in Unity URP](https://youtu.be/RMt6DcaMxcE?si=WI7H5zyECoaqBsqF)
    - [MinionsArt](https://www.youtube.com/@MinionsArt)
        - [Toon Shader Tutorial](https://www.youtube.com/watch?v=FIP6I1x6lMA&ab_channel=MinionsArt)
    - [Brackeys](https://www.youtube.com/@Brackeys)
        - [Intro to Unity Shader Graph](https://www.youtube.com/watch?v=Ar9eIn4z6XE&ab_channel=Brackeys)
    - [Robin Seibold](https://www.youtube.com/@RobinSeibold)
        - [Tutorial on Depth and Normal Buffer Robert's Cross Outliens in Unity](https://youtu.be/LMqio9NsqmM?si=zmtWxtdb1ViG2tFs)
    - [Alexander Ameye](https://ameye.dev/about/)
        - [Article on Edge Detection Post Process Outlines in Unity](https://ameye.dev/notes/edge-detection-outlines/)
