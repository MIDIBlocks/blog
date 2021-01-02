# Introducing Handsfree.js - Integrate hand, face, and pose gestures to your frontend 🖐👀🖐

![Handsfree.js](https://media1.giphy.com/media/BBcnSU1IJ5tpQbwXDI/giphy.gif)

Hello and thank you for checking out this blog, I'm so excited to share what I've been working on! But before I dive into talking about what Handsfree.js is and how you can get started, I thought I'd show you some things I've made with it first!

---

## Examples

### Use it to trigger events

Just today I released the [Pincher Plugin](https://handsfree.js.org/ref/plugin/pinchers.html) which emits 24+ pinching events with 3 states - `start`, `held`, `released` - for pinching with your index, middle, ring, and pinky fingers. It's modelled after the Mouse Events and you can listen to them similarly with `document.addEventListener()`: https://handsfree.js.org/ref/plugin/pinchers.html

![Pincher Plugin](https://media3.giphy.com/media/IHcXdVDrnpVnZqwq4z/giphy.gif)

### Use it to scroll pages handsfree

Here's a Browser Extension I'm working on that helps you scroll websites handsfree. It uses the [MediaPipe Hands model](https://handsfree.js.org/ref/model/hands.html) to track your hands. This GIF was actually really easy to make with the built in `pinchScroll plugin`, which enables this customizable functionality in a single line of code: https://handsfree.js.org/ref/plugin/pinchScroll.html

![Handsfree Browser Extension](https://media0.giphy.com/media/BSkodGjuwBPAEwxjGv/giphy.gif)

### Use it to create new kinds of Assistive Technologies

This is one of my favorites, and it uses the ["Face Pointer" plugin](https://handsfree.js.org/ref/plugin/facePointer.html) to allow you to move a pointer with your face, scroll pages, and click on things. It's powered by the [Jeeliz Weboji model](https://handsfree.js.org/ref/model/weboji.html)

![Face Pointers](https://media0.giphy.com/media/Iv2aSMS0QTy2P5JNCX/giphy.gif)

### Use it to control desktop games

Here's me playing "Into the Breach" on my desktop with Face Pointers and Hand Pointers. These are super easy to make too, all I did was use the Face and Hand Pointer plugins and then stream them over to my desktop with [Robot.js](https://robotjs.io/) to trigger native mouse events:

![Face Pointers Into the Breah](https://media1.giphy.com/media/eABiZprIEtouRZIc75/giphy.gif)

![Hand Pointers Into the Breach](https://media0.giphy.com/media/pdDOkUpnRbzMk8r0L4/giphy.gif)

![Handsfree Solitaire](https://media2.giphy.com/media/YATR9GZSSHKeNw3fht/giphy.gif)

### Use it to make your own games

But why just play games when you can make them too!? Here are a few games I've made, which I plan on grouping together into a "Mario Party" like game where you roll dice to move on a board and then play minigames with your friends at the end of each round.

Here is "DuckFace Hunt", "Flappy Pose", and "Handsfree Jenga":

![DuckFace Hunt](https://media2.giphy.com/media/MWvfeCGV2MYmaRzvkP/giphy.gif)

![Flappy Pose](https://media2.giphy.com/media/hwNj7nfkDljmlnaNRA/giphy.gif)

![Handsfree Jenga](https://media4.giphy.com/media/brC1Ow2v62htVmpfLh/giphy.gif)

### Use it to control robots and drones

Of course, you're not limited to controlling things on the web or even desktop. With WebSockets you control anything connected to your computer, like this Universal Robot which I tried to puppeteer with my own head:

![Handsfree robot](https://media2.giphy.com/media/azwwFNLRXmZ1WnRzFT/giphy.gif)

![Handsfree UR5](https://media4.giphy.com/media/BHdfIcCsGCNlIAnKD7/giphy.gif)

### Use it for art, music, and other experiences

There's so much more that you can do! Here are some other experiments like my Handsfree Psychedelic Art Maker and WebXR DevTools so that you can work on WebXR apps without XR equipment:

![Diffusionist](https://media0.giphy.com/media/erAsyoAJukeBfS9ila/giphy.gif)

![Handsfree A-Frame](https://media0.giphy.com/media/YOPrRX6vTy6tb3frgt/giphy.gif)

![Handsfree WebXR](https://media2.giphy.com/media/w3JUFtNyXNafLVrh6F/giphy.gif)

---

## Getting Started

### Initializing and starting Handsfree

The easiest way to get started is with a CDN. You can create an HTML file and copy/paste this in without the need for a server:

```html
<head>
  <!-- Import helper classes and styles -->
  <link
    rel="stylesheet"
    href="https://unpkg.com/handsfree@8.1.1/build/lib/assets/handsfree.css" />
</head>
<body>
  <!-- Import Handsfree.js in body (as it adds body classes) -->
  <script src="https://unpkg.com/handsfree@8.1.1/build/lib/handsfree.js"></script>

  <script>
    // Use the hand with defaults
    handsfree = new Handsfree({hands: true})

    // Start webcam and tracking (personally, I always like to ask first)
    handsfree.start()
  </script>
</body>
```

You can also use import with NPM. By default this will still load the models from a CDN as they are quite large (some are over 10Mb), but I have instructions for ejecting the models into your assets folder here: [https://handsfree.js.org/#hosting-the-models-yourself](https://handsfree.js.org/#hosting-the-models-yourself)

```bash
npm i handsfree
```

```js
handsfree = new Handsfree({
  // Use the hand model with custom config
  hands: {
    // Always make sure to enable them
    enabled: true,
    
    // Let's track up to 4 hands. It's best to be kind and ask permission first tho!
    maxNumHands: 4,
  }
})

// Start webcam and tracking (personally, I always like to ask first)
handsfree.start()
```

### Working with the data

There are two main ways to work with Handsfree.js. My preferred way is to create plugins using [handsfree.use(newPluginName, callback)](https://handsfree.js.org/ref/method/use.html). I call them plugins because they "plug into" the main loop that's started when you run `handsfree.start()`.

Plugins run their `callback` on every webcam frame and receive all the data from all the running computer vision models. Here's a very simple plugin that simply console logs data:

```js
// Let's use our hands and face
handsfree = new Handsfree({hands: true})
handsfree.start()

// Let's log the data
handsfree.use('logger', (data) => {
  console.log(data.hands)
})
```

If you need more advanced functionality then you can pass an object with specific hooks that'll run during various phases of the plugin. For example:

```js
handsfree.use('advancedLogger', {
  
})
```

## Outline
- What is Handsfree.js
- Examples
- Getting started
- Roadmap and goals
- Discussion, Social Media, and Discord links