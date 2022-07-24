---
title: "Controlling Anki with a Gamepad"
date: 2022-07-22T23:59:34-07:00
tags: ["macOS", "Anki"]
---

## Background

Recently I have started using an open-source flashcard program called [Anki] to
learn Chinese vocabulary. I may write more about Anki someday, but the most
important feature of Anki is that it is designed around the concept of [Spaced
Repetition]. Every day, you study new cards and also review existing cards, and
rate how well you know each card using one of the `Again`, `Hard`, `Good`, and
`Easy` buttons.

[Anki]: https://apps.ankiweb.net/
[Spaced Repetition]: https://supermemo.guru/wiki/Spaced_repetition

For each card, at a minimum you need to first select the `Show Answer` button
(to see the back of the flashcard) and then one of the difficulty buttons. With
review sessions that can sometimes span hundreds of cards, controlling all of
this with a mouse can be tiresome. Fortunately, the entire learn and review
flow can be managed with keyboard shortcuts. Some users[^1] have noticed that
even the ergonomics of using a keyboard to control Anki are not the best.
Because of this, there has been a recent trend of experimenting with using
video game controllers to control your Anki review.

[^1]: Mostly these are kids in medical school that are studying far more
	intently and for far longer than I am.

## My Setup

Back when I was really into video games, I experimented with pairing all
different kinds of controllers to my laptop. One of the video game controllers
I have lying around is a SNES30[^SNES30], which is a bluetooth controller designed to
emulate the look and feel of a SNES controller made by a company called
[8bitdo]. 8bitdo controllers are cool because they are designed with the goal
that you can use it anywhere, so they come with a variety of different
operating modes for different kinds of devices.

[8bitdo]: https://www.8bitdo.com/
[zero2]: https://www.8bitdo.com/zero2/

[^SNES30]: It looks like the SNES30 I have is out of stock, but other (less
	retro-looking) 8bitdo controllers like the
	[zero2] come with the same functionality.

Anki does not natively support video game controllers[^native support], but
since it does come with keyboard shortcuts, the approach most users take is to
map buttons on the controller to keyboard keys to execute the various Anki
functions. On macOS, there are a few apps that map joystick buttons to keyboard
keys, but when I was experimenting a few years ago for an unrelated project I
did not have a ton of luck with any of them. Fortunately for me, since 8bitdo
tries to support so many kinds of devices, one of the operating modes for my
SNES30 is bluetooth keyboard emulation, which you can trigger by holding
`Select + B` for 3 seconds when turning on the controller.

[^native support]: There are various plugins advertising controller support,
	but I have not experimented with them.

There is one final wrinkle, however: Anki does not natively support rebinding
keyboard shortcuts, and even if it did I want to keep the default shortcuts so
that I can still review with the keyboard sometimes too. To solve this, I used
an open-source macOS application called [Karabiner Elements]. Karabiner
Elements is an extremely powerful macOS app that intercepts keyboard commands
and lets you rebind keys into other keys, key combinations, or even macros.
Furthermore, Karabiner Elements can differentiate between keys pressed on
different keyboards, so it knows when I am pressing buttons on my SNES30 and
can handle those differently than the key events from my other keyboards. I use
Karabiner Elements to intercept the key events from some of the buttons my
SNES30 and replace those with the keyboard shortcuts for the relevant Anki
functions.

[Karabiner Elements]: https://karabiner-elements.pqrs.org/

Karabiner Elements stores its mappings in a JSON config file, so you can put it
under version control or share it with other people. The relevant part of my
config file is below:

```json
{"devices": [
  {
    "disable_built_in_keyboard_if_exists": false,
      "fn_function_keys": [],
      "identifiers": {
        "is_keyboard": true,
        "is_pointing_device": false,
        "product_id": 10304,
        "vendor_id": 11720
      },
      "ignore": false,
      "manipulate_caps_lock_led": true,
      "simple_modifications": [
        { "from": { "key_code": "g" }, "to": [ { "key_code": "spacebar" } ] },
        { "from": { "key_code": "i" },
          "to": [ { "key_code": "r", "modifiers": [ "left_shift" ] } ]
        },
        { "from": { "key_code": "j" },
          "to": [ { "key_code": "2", "modifiers": [ "left_shift" ] } ]
        },
        { "from": { "key_code": "k" }, "to": [ { "key_code": "1" } ] },
        { "from": { "key_code": "m" }, "to": [ { "key_code": "3" } ] }
      ]
  }
]}
```
