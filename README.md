# Safari Smarts

A bilingual (English / Kiswahili) numeracy and literacy learning game for children aged 5 to 10, built around Tanzania: its animals, places, money, food and proverbs.

Everything is one self-contained web page. There is no server, no account sign-up, no tracking and no internet needed after the page loads (apart from the optional Google Fonts).

---

## What a child sees

1. **Pick or create a child.** Each child has a name, age, language, animal buddy and teacher. Several children can share one device and switch between profiles.
2. **Take a quick pre-test (optional).** It asks a couple of questions per chapter and stops at the first mistake. Chapters before that point are marked as skipped, so each child starts where they belong. "Start from the beginning" is always available.
3. **Learn along a map.** Each subject is a trail of chapters named after Tanzanian places (Serengeti, Zanzibar, Kilimanjaro and more). Every chapter has 4 lessons and then a boss fight.
4. **Earn things.** Correct answers give coins. Answers in a row build a combo for bonus coins. Coins buy blocks, buddy accessories and boosts. Tanzanite gems buy rarer items.
5. **Play.** Five minigames, a sticker book, and a village (My Boma) to build.

## Subjects and chapters

| Track | Chapters |
|---|---|
| **Numbers** (11) | Count to 10, Count to 20, Numbers to 100, Shapes and patterns, Adding, Taking away, Tens and hundreds, Shillings and money, Time and measuring, Times tables, Sharing and fractions |
| **Kiswahili** (9) | Letters, Vowels and first letters, Syllables, Words and pictures, Spelling, Sentences, Reading stories, Opposites and plurals, Proverbs (methali) |
| **English** (9) | The alphabet, Letter sounds, Build 3-letter words, Picture words, Spelling, Sentences, Opposites and plurals, Reading stories, Grammar and punctuation |

Questions are generated, not fixed lists, so they vary each time. There are four difficulty steps within every chapter.

## Features

**Learning**
- Three ways to answer: tap a choice, type a number or letter (on-screen keypad or physical keyboard), or tap tiles to build a word or sentence.
- **Two-tap answering:** the first tap selects and reads the answer aloud, the second tap submits. A parent can turn this off.
- Hints, and a retry queue that brings missed questions back before the lesson ends.
- Review practice built from a child's own mistakes.
- Daily Safari: a mixed challenge each day, plus a daily goal (1 to 5 lessons) with a reward chest.
- Streaks with streak freezes and milestones.

**Teacher and accessibility**
- Spoken questions in English and Kiswahili using the device's built-in voices.
- Choose between three teachers (Mwalimu Twiga, Tembo and Kima), each with its own speed and pitch, and pick the voice per language.
- Subtitles with the spoken word highlighted.
- Sound and music toggles, and a calm mode that reduces animation. Reduced-motion settings are respected.
- Full interface in English or Kiswahili, switchable at any time.

**Rewards and play**
- **Boss fights:** a boss has 6 HP, the child has 3 hearts, and a combo of 3 or more unlocks a super attack. Six Tanzanian-themed bosses.
- **Six animal buddies** (lion, giraffe, elephant, hippo, zebra, monkey) with mood animations and wearable accessories.
- **Duka shop:** 16 block types, accessories, streak freezes and sticker packs.
- **My Boma:** an 8 by 6 grid to build a village with the blocks you bought.
- **Sticker book:** about 50 stickers in three rarities, with fun facts and a pack-opening animation.
- **13 medals** for milestones.
- **Minigames:** Balloon Pop, Race to Kilimanjaro, Memory Match, Word Builder and Sokoni Market (pay exact prices with shilling coins).

**Saving**
- Autosave after every action and when the page is closed or hidden. A "Saved" chip in the header shows the state.
- A lesson interrupted halfway resumes at the same question.
- If the browser blocks storage, the app keeps working in memory and shows a notice.

## For parents and teachers (Grown-ups area)

Open it from the **Me** tab. A simple addition question keeps small children out.

- Progress report per child: chapters finished per subject, accuracy, topics that need practice, topics going well.
- Set the daily goal for each child.
- **Back up and restore:** copy a code that holds every child's progress and paste it on another device or browser to restore it.
- Reset a child's progress or delete a profile.

## Things to know

- **Progress lives in the browser on that device.** It does not sync between devices. Use the backup code to move it. Clearing site data erases it, so make a backup first.
- **Kiswahili speech depends on the device.** Many phones and computers have no Kiswahili voice installed. Subtitles still work, and a voice can be added in the device's text-to-speech settings.
- **Pre-test placement is quick, not diagnostic.** It stops at the first mistake. Use the progress report to see real strengths and gaps.
- The design is based on a feature list, not on the reference sites themselves, because those sites could not be opened during the build.

## For developers

Plain HTML, CSS and JavaScript with no libraries and no build dependencies beyond Python 3.

```
src/
  shell.html    page skeleton, title, fonts, mount points
  styles.css    design system and all screens
  art.js        SVG animals, bosses, blocks, shapes, clock, icons
  data.js       translations, vocabulary, stories, chapters, stickers, shop, medals
  gen.js        question generators for every chapter (GEN.n1 .. n11, k1 .. k9, e1 .. e9)
  audio.js      sound effects, music, speech, subtitles
  state.js      profiles, autosave, streaks, economy, backup codes
  ui.js         screen router, profile screens, home and Learn tab
  lesson.js     lesson, pre-test and boss session engine
  tabs.js       Play, Stickers, Duka shop, Me, grown-ups area
  games.js      the five minigames
  main.js       start-up
build.py        joins everything into dist/
dist/
  artifact.html the page as published
  index.html    the same page as a full HTML document, for opening locally or hosting
  test.html     test build that exposes window.__dbg (not for release)
```

Build:

```
python3 build.py
```

Open `dist/index.html` in a browser to run it locally, or upload that one file to any static host.

**Architecture in short**
- One script wrapped in a single function. Screens are plain functions that return HTML, drawn into one container.
- Buttons use `data-act` attributes and one shared click handler, so there are no per-button listeners.
- Global state: `S` (interface), `DB` (all saved data), `P` (active child), `L` (current lesson), `G` (current minigame).
- Data is saved to `localStorage` under the key `safari-smarts-v1`.
- Add a chapter by adding an entry to `CH` in `data.js` and a matching generator in `gen.js`.

**Testing.** The question generators were checked with a stress test of more than 9,000 generated questions. Screens and flows (profile creation, pre-test, lessons, boss fights, shop, minigames, parent area, Kiswahili mode, resume after reload, tablet width) were checked in a headless browser with no console errors.
