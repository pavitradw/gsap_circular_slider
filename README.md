# gsap_circular_slider

CardSlider - Rotating Card Animation

Overview

The CardSlider is a JavaScript class that creates a rotating card slider effect using GSAP (GreenSock Animation Platform). It dynamically rotates a set of card images in a smooth, circular motion while changing the background color at each step.

Features

Uses GSAP for smooth animations.

Dynamically changes the background color during transitions.

Supports multiple instances on the same page.

Fully customizable transition speeds and effects.

Automatically loops animations infinitely.

Demo

A live preview of the animation can be added here.

Installation & Usage

1. Include the Required Libraries

Ensure you have GSAP included in your project. You can add it via a CDN:

<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/gsap.min.js"></script>

2. HTML Structure

Wrap your images or cards inside a container with a unique ID:

<section id="cardSlider1" class="slider-section">
  <div class="deck">
    <img src="logo1.png" class="logo" />
    <img src="logo2.png" class="logo" />
    <img src="logo3.png" class="logo" />
    <img src="logo4.png" class="logo" />
    <img src="logo5.png" class="logo" />
  </div>
</section>

3. JavaScript Implementation

Include the CardSlider JavaScript class and initialize it:

class CardSlider {
  constructor(sectionId) {
    this.section = document.getElementById(sectionId);
    this.logos = gsap.utils.toArray(`#${sectionId} .logo`);
    this.deckLogos = gsap.utils.toArray(`#${sectionId} .deck .logo`);
    
    this.colors = ['#F5E6E8', '#D5F5E3', '#E8F6F3', '#F5EEF8', '#FDF2E9', '#F4ECF7', '#E8F8F5'];
    this.currentIndex = 0;
    this.init();
  }
  
  init() {
    this.deckLogos.forEach((logo, index) => {
      gsap.set(logo, {
        zIndex: index,
        rotationY: index * 1,
        transformOrigin: "50% 750px",
      });
    });
    this.runAnimation();
  }
  
  changeBackgroundColor() {
    this.section.style.backgroundColor = this.colors[this.currentIndex];
    this.currentIndex = (this.currentIndex + 1) % this.colors.length;
  }
  
  runAnimation() {
    gsap.to(this.deckLogos, {
      duration: 2,
      ease: "power2.out",
      rotation: (index) => (360 / this.deckLogos.length) * index,
      transformOrigin: "50% 750px",
      opacity: 1,
      stagger: 0.1,
      onComplete: () => this.startRotationCycle()
    });
  }
  
  startRotationCycle() {
    const cardRotationTime = 5;
    const wrapUpTime = 3;
    
    const tl = gsap.timeline({
      onComplete: () => {
        gsap.set(this.deckLogos, { rotation: 0, opacity: 0 });
        this.runAnimation();
      }
    });
    
    this.deckLogos.forEach((_, index) => {
      tl.to(this.deckLogos, {
        rotation: "+=" + (360 / this.deckLogos.length),
        duration: cardRotationTime,
        ease: "power2.inOut",
        onStart: () => this.changeBackgroundColor()
      });
    });
    
    tl.to(this.deckLogos, {
      rotation: (index) => {
        const currentRotation = gsap.getProperty(this.deckLogos[index], "rotation");
        return Math.ceil(currentRotation / 360) * 360;
      },
      duration: wrapUpTime,
      ease: "power3.inOut",
      stagger: { amount: 0.5, from: "end" }
    });
    
    tl.to(this.section, { backgroundColor: this.colors[0], duration: 1 }, "-=2");
  }
}

// Initialize the slider after DOM loads
document.addEventListener('DOMContentLoaded', () => {
  new CardSlider('cardSlider1');
});

Customization

Option

Description

sectionId

The ID of the container section where the slider should be applied.

this.colors

List of background colors that cycle during animation. Modify as needed.

rotation

Controls the degree of rotation for each card. Adjust for different effects.

cardRotationTime

Time taken for each card to rotate (default: 5 seconds).

wrapUpTime

Duration of the final wrap-up animation before restarting (default: 3 seconds).

Dependencies

GSAP (GreenSock Animation Platform)

License

This project is licensed under the MIT License.

Contact

For any issues or improvements, feel free to contribute or reach out!
