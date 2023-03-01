---
layout: page
title: Stay Hydrated!
toc: true
---

<iframe width="560" height="315" src="https://www.youtube.com/embed/SpqKJUrZnv0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>


<table>
  <tbody>
    <tr>
      <td>Entry</td>
      <td>Global Game Jam 2023</td>
    </tr>
    <tr>
      <td>Developer</td>
      <td>Team Hydrogen</td>
    </tr>
    <tr>
      <td>Year</td>
      <td>2023</td>
    </tr>
    <tr>
      <td>Role</td>
      <td>Programmer, Designer, VFX</td>
    </tr>
    <tr>git 
      <td>Tech</td>
      <td>Unreal Engine 5</td>
    </tr>
    <tr>
      <td>Platform</td>
      <td>Windows</td>
    </tr>
    <tr>
      <td>Download Page</td>
      <td><a href="https://makotoichinose.itch.io/hydrogen">itch.io</a></td>
    </tr>
  </tbody>
</table>

This was my first ever game jam that I attend on-site, and managed to finish and submit just in time.

## Code

In this project, I handle all the programming in the game, including:
### Main menu
![](https://img.itch.zone/aW1hZ2UvMTkwODIwNy8xMTIyNDk3MS5qcGc=/original/cX1NF3.jpg)

A fairly standard main menu preceded by a splash screen, programmed with UMG.

### Cinematics

![](https://img.itch.zone/aW1hZ2UvMTkwODIwNy8xMTIyNDk2OC5qcGc=/original/htll8w.jpg)

Also fairly standard cinematic playback upon starting the game, programmed with UMG and Sequencer.

### Gameplay Loop

![](https://img.itch.zone/aW1hZ2UvMTkwODIwNy8xMTIyNDk3Mi5qcGc=/original/HpdwMp.jpg)

The gameplay loop consists of guiding the root to reach the pool within the move count. Before making a move, it checks if there's still remaining moves at all, then check for barriers and existing grown roots are checked with line trace forward to the direction, and check for any kind of triggers like the pool to trigger level completion.

If the move deemed valid from aforementioned check, a "mover" actor moves to adjacent grid and changing the sprite by means of switching the flipbook index in material, thus making a move. If it moves away from the core, a root trunk actor is created where the "mover" actor was last located, and determining the sprite used by comparing last move direction to the input direction, allowing for bend trunks.

To finish a level, player must reach the pool with 0 moves left. This was done by checking the remaining moves and aforementioned trigger using the "mover" actor. The core actor also switched the sprite to a fully grown tree upon completion.

### VFX

I also handle some visual effects in the game, including the fake cloud shadows, cloud borders, and the cinematic comet intro.

## Mistakes

Mistakes were made that I realized after the jam: first, I didn't managed to write the tutorial on time. This caused some confusion among players the moment it was up for showcase. I also haven't destroyed the UI upon changing level, causing overlap on the timer box.

The latter caused some side visual effect of those found on digital displays. This could be made intentional.

