---
layout: post
title: Building a Raspberry Pi Presence Light for Teams & Zoom
description: How to build a Raspberry Pi Presence Light that works with Teams & Zoom
---

My partner and I both work from home and spend large parts of our days on video calls. In order to avoid disturbing one another, we wanted some way to automatically signal when we are in meetings, particularly when we are presenting.

My company uses Teams, whereas my partner’s company uses Zoom, so we needed to design a solution that worked for both services.

<div style="float: right; width: 50%; margin: 20px">
  <img alt="busy light animation" src="{{ site.baseurl }}/assets/img/raspberry-pi-presence-light/busy-light-animation.gif" />
  <p style="text-align: center">Animation courtesy of <a href="https://www.jean-liu.com" target="_blank" rel="noopener noreferrer">Jean Liu</a></p>
</div>

Inspired by a number of Raspberry Pi builds on the Internet (<a href="https://www.eliostruyf.com/diy-building-busy-light-show-microsoft-teams-presence" target="_blank" rel="noopener noreferrer">here</a>, <a href="https://www.hackster.io/maexbert/teams-presence-for-raspberry-pi-d2a63c" target="_blank" rel="noopener noreferrer">here</a>, and <a href="https://www.michaelleo.com/zoom-busy-light" target="_blank" rel="noopener noreferrer">here</a>), I’ve made a set of lights that synchronise with our Teams and Zoom presence statuses throughout the day. To keep things clear and simple, I’ve mapped the statuses to three colours:

![#00fa00](https://via.placeholder.com/15/00fa00/000000?text=+) means that the user is available\\
![#ff6cb4](https://via.placeholder.com/15/ff6cb4/000000?text=+) means that the user is in a meeting\\
![#fa0000](https://via.placeholder.com/15/fa0000/000000?text=+) means that the user is presenting

We’re really happy with the result, which has meant that it’s easier than ever to see when the other person is free for a quick chat or a coffee in between meetings. As this was my first Raspberry Pi project, I learned a lot and thought that it would be useful to share my experience.

## What you’ll need

I need to start this section with a major caveat.

After starting to write this post, I noticed that one of the key components, the Unicorn pHAT, was discontinued. It’s been replaced by the manufacturer with a new version, the Unicorn HAT Mini. Since the Unicorn HAT Mini pixels and driver chip are different to the previous Unicorn boards, this changes my project instructions slightly. It might also be necessary to make a few changes to run any Python code that talks to the Unicorn HAT Mini.

For clarity, I have included 2 lists of parts: 1) my original build and 2) a suggested build using parts that are currently available.

### Original build

- <a href="https://thepihut.com/products/raspberry-pi-zero-w" target="_blank" rel="noopener noreferrer">Raspberry Pi Zero W</a> - A tiny headerless Raspberry Pi with WiFi and Bluetooth
- <a href="https://shop.pimoroni.com/products/unicorn-phat" target="_blank" rel="noopener noreferrer">Unicorn pHAT</a> - An LED board that attaches on top of the Raspberry Pi <span style="color:red">(discontinued)</span>
- <a href="https://thepihut.com/products/pogo-a-go-go-solderless-gpio-pogo-pins" target="_blank" rel="noopener noreferrer">Pogo-a-go-go Solderless GPIO Pogo Pins</a> - A solderless solution for connecting the Pi and pHAT (only 3/10 pins required, install them upside-down!)
- <a href="https://thepihut.com/products/phat-diffuser" target="_blank" rel="noopener noreferrer">pHAT Diffuser</a> - A diffuser to sit on top of the LEDs <span style="color:orange">(optional)</span>
- <a href="https://thepihut.com/products/pibow-zero-w" target="_blank" rel="noopener noreferrer">Pibow Zero W case</a> - A case for the Raspberry Pi Zero W <span style="color:orange">(optional)</span>

**Note**: _I've listed the pHAT Diffuser as optional because you can change the Unicorn's LED brightness programmatically. The Pibow Zero W case is optional too, as it is largely unused if you decide to use the pHAT Diffuser on top of the Unicorn pHAT_.

<img width="75%" align="center" style="display: block; margin-left: auto; margin-right: auto;" alt="busy light photo" src="{{ site.baseurl }}/assets/img/raspberry-pi-presence-light/busy-light-photo.jpg" />

### Suggested build (which I’ve not tested)

Keep in mind that the parts listed below weren’t used in my original build, but I’m including them here to provide a starting point for anyone who wants to replicate the project but can’t get the Unicorn pHAT.

Use this build with caution, as I haven’t tried it myself. However, I’d be interested to hear from anyone who has attempted this project with the suggested hardware. I’ll update this post if there are any issues.

This build should also not require soldering - since the Unicorn HAT Mini comes with a female header pre-attached, I’m assuming you can just buy a Raspberry Pi Zero WH (which includes a pre-soldered male header) to plug into it directly.

- <a href="https://thepihut.com/products/raspberry-pi-zero-wh-with-pre-soldered-header" target="_blank" rel="noopener noreferrer">Raspberry Pi Zero WH</a> - A tiny Raspberry Pi with WiFi and Bluetooth
- <a href="https://thepihut.com/products/unicorn-hat-mini" target="_blank" rel="noopener noreferrer">Unicorn HAT Mini</a> - An LED board that attaches on top of the Raspberry Pi
- <a href="https://thepihut.com/products/phat-diffuser" target="_blank" rel="noopener noreferrer">pHAT Diffuser</a> - A diffuser to sit on top of the LEDs <span style="color:orange">(optional - see note in original build)</span>
- <a href="https://thepihut.com/products/pibow-zero-w" target="_blank" rel="noopener noreferrer">Pibow Zero W case</a> - A case for the Raspberry Pi Zero W <span style="color:orange">(optional - see note in original build)</span>

## The solution

### TL;DR

Follow the main respository readme <a href="https://github.com/jonathanwelton/raspberry-pi-presence-light-for-teams-and-zoom" target="_blank" rel="noopener noreferrer">here</a>.

#### For Teams

- Configure an Azure AD app to access your Teams presence following <a href="https://docs.microsoft.com/en-us/azure/active-directory/develop/quickstart-register-app" target="_blank" rel="noopener noreferrer">this guide</a>
- Install the <a href="https://github.com/jonathanwelton/raspberry-pi-presence-light-for-teams-and-zoom" target="_blank" rel="noopener noreferrer">Raspberry Pi Presence Light for Teams & Zoom</a> app on your Raspberry Pi

#### For Zoom

- Create a Zoom webhook app following <a href="https://marketplace.zoom.us/docs/guides/build/webhook-only-app" target="_blank" rel="noopener noreferrer">this guide</a>
- Configure and deploy the <a href="https://github.com/jonathanwelton/zoom-presence-indicator-api" target="_blank" rel="noopener noreferrer">Zoom Presence Indicator API</a> to receive the Zoom presence events
- Install the <a href="https://github.com/jonathanwelton/raspberry-pi-presence-light-for-teams-and-zoom" target="_blank" rel="noopener noreferrer">Raspberry Pi Presence Light for Teams & Zoom</a> app on your Raspberry Pi

---

Towards the end of last year, I found a blog post by <a href="https://www.eliostruyf.com/diy-building-busy-light-show-microsoft-teams-presence" target="_blank" rel="noopener noreferrer">Elio Struyf</a> on building a presence indicator for Microsoft Teams using a Raspberry Pi, and it piqued my interest.

Elio’s solution relies on hosting your own Homebridge server, with a custom Homebridge plugin to poll the Microsoft Graph API for presence changes. Any presence change will then be forwarded to an API running on the Pi to update its colour.

However, I felt that a simpler solution would better suit my needs, given HomeKit integration wasn’t a requirement.

<a href="https://www.hackster.io/maexbert/teams-presence-for-raspberry-pi-d2a63c" target="_blank" rel="noopener noreferrer">Maxi Krause</a> went on the same journey, and their solution entirely removes the need for a Homebridge instance by pushing the polling logic down into a single app running on the Pi. Maxi's solution does require that you configure your own Azure AD app, but other than that, it was exactly what I wanted, and seemed to work as well as Elio’s solution.

I just needed to adapt it for my requirements, and create a matching implementation for Zoom.

Thankfully, Zoom utilises webhooks, so any presence events can be fired at a specified URL. However, given the Pi wouldn’t be visible outside my local network I created a public API it could call to use as a proxy.

Using the <a href="https://github.com/zoom/zoom-sample-webhookapp" target="_blank" rel="noopener noreferrer">Sample Zoom Webhook App</a> as a starting point, and taking a few pointers from <a href="https://www.michaelleo.com/zoom-busy-light" target="_blank" rel="noopener noreferrer">Michael Leo</a>, I modified it to receive Zoom presence events and place them in local storage.\*

Instead of calling an Azure AD app, the Zoom version of the Pi app calls this API to retrieve the presence and then matches the response against a set of Zoom presence statuses.

For a full set of instructions to replicate my solution, please follow the guide included as part of the <a href="https://github.com/jonathanwelton/raspberry-pi-presence-light-for-teams-and-zoom" target="_blank" rel="noopener noreferrer">respository readme</a>. It's worth noting that when you configure the <a href="https://github.com/jonathanwelton/zoom-presence-indicator-api" target="_blank" rel="noopener noreferrer">Zoom Presence Indicator API</a> you can omit any of the values for the Azure IoT Hub and it will still operate.

If you decide to try this project out, good luck, and please let me know how it goes!

\* <sub>My original intention was to push these events to an Azure IoT Hub, which I got working with this API. However, <a href="https://github.com/microsoftgraph/nodejs-webhooks-rest-sample/issues/175" target="_blank" rel="noopener noreferrer">issues</a> implementing the same solution for the Microsoft Graph API resulted in falling back on long polling for both solutions, as a first pass. If I make progress with the aforementioned issue, I may try and implement it that way again.</sub>
