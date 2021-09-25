# hello-world
Just another repository

Hello Git Community!

I am Jeff, new here. But __*not*__ new to tech and coding. Python predominantly, and beginning with TinyML and Arduino so far, so cpp is also going to be in the mix.

## Documenting my activity:

My process:

1. **Git** Practicing with Repositories, Branches, Commits, Pull Requests, and Merges with [help from this Guide](https://guides.github.com/activities/hello-world/)
2. **Markdown** Learned to use [Markdown from this guide](https://guides.github.com/features/mastering-markdown/)
3. **TinyML** Took course for TinyML from EdX (complete)
1. Define the parts of golf swing idea (ongoing)

Next steps:
* Upon completion of EdX course, review the material and do all assignments again
* Create **Git Page** with [**this** guide](https://guides.github.com/features/pages/) to show Accomplishments
* Add Accomplishments to **Page**
  * Took Class
  * Bought book
  * Document assignments or examples
  * Include photos of certs, book, Arduino board
    * make GIF of these
* Determine whether **Page** should be redirected here from **tech**.jeffreysorgen.com 
* OPTIONAL: Create **Wiki** showing Accomplishments
* Create new repository "hello-world-arduino"
* Jot down ideas for projects here
  * wind turbine
    * is the popular product TinyML or IoT ?
  * Continue to flesh out golf swing idea, _below_

### Golf Swing Idea

Uses IMU:
* IMU void loop waits until motion halts, orientation on plane, sets up to record swing motion (event loop)
* MCU records motion event
  - stores motion data
  - engages KWS loop 

Uses KWS:
- KWS void loop listens for 'yes'
- null records label then returns to IMU void loop
- 'yes' records label then returns to IMU void loop

Uses BLE:
- data fills available memory with specific number of motion events
- device connects with BLE and prompts signal from smartphone
- waits to connect
- drops data off
- clears data out
- returns to IMU void loop



*All I'm doing right now is working with the hello-world readme.md file to practice using Git. After building confidence just with the MD file, I'll start working with CODE*
