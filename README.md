# Continue Experience

I've now been using Continue as my VSCode AI for several days while building [Platy](https://github.com/djgould/platy) an AI meeting transcription and summarization tool I started working on a few days ago.

## the project

Platy is current in it's early stages, built on Tauri, a cross platform app framework that uses a Rust backend and a NextJS frontend. My vision for Platy is something similar to otter.ai or Krisp, focused on meeting transcription and summarization, powered entirely by on device AI models. This means all meeting data can stay on your device!

Given Platy has only been around for a couple days, the current version is just a simple stateless recording tool, with live transcription powered by [whisper.cpp](https://github.com/ggerganov/whisper.cpp)

![2024-05-16 21 27 59](https://github.com/djgould/continue-experience/assets/6018174/d6160fcd-2832-4b2a-b648-928b524ef2cd)

## the setup

Building on Tauri for me meant two things:
1). I could use NextJS and React for the frontend, two tools I am very familiar with.
2). The lightweight rust backend enables interfacing with low level libraries for things like interacting with audio devices, the filesystem, and whisper.cpp

The one problem with that is I don't know Rust, and haven't worked with c/c++ or anything similar since college. Fortunately we have AI to the rescue and I certainly would not have been able to make the progress I have so quickly without using Continue!

## highlights

Continue came in handy especially for a few specific functions:
- [transcribe_wav_file](https://github.com/djgould/platy/blob/main/src-tauri/src/recorder.rs#L309-L401)
- [combine_segments](https://github.com/djgould/platy/blob/main/src-tauri/src/recorder.rs#L100-L162)
- [get_complete_transcription](https://github.com/djgould/platy/blob/main/src-tauri/src/lib.rs#L275-L325)
- [get_realtime_transcription](https://github.com/djgould/platy/blob/main/src-tauri/src/lib.rs#L224-L272)

Contiues chat tool was by far the most useful part for me. While learning a new language it is extremely helpful to grab code snippets I don't undertand. Rust introduces programming paradigms that are very different from other languages, and it is difficult to make any progress without understanding them. Being able to go from `cannot move out of borrowed content` and being able to CMD + L and ask "Can you explain borrowing in rust?" with a few follow up questions closes the learning loop significantly.

The code completion with starcoder:7b while developing the frontend was also incredibly seemless. Continue did a great job suggesting useful code snippets and I found myself quickly tabbing my way through a functional UI. Perhaps more importantly I found it did a good job _not_ suggesting snippets when they werent needed, which is one of the more annoying aspects of many coding copilot tools.

## lowlights

AI is really bad at rust. I am too but I was genuinly surprised by how often it suggested code that was completely broken for even small simple code snippets. Rust has very strict typing and while the code logic seemed appropriate, the suggested snippets broke all of rusts rules around ownership, borrowing and mutability, and required major revision. Better than nothing I suppose but still annoying.

This also meant the tabcomplete code snippets were pretty useless most of the time. This is annoying and I found myself disabling Continue tabcomplete when editing Rust files and enabling again when editing typescript files. If it doesn't already exist it would be nice to configure Continue to only autocomplete in specified language types.

I don't think this is only a problem with copilot, even going to ChatGPT and asking it for help for Rust related things gave me pretty poor results. It's great at explaining high level rust concepts, but bad at implementing them.

## ideas

- Start a new session without clearing all input above. I find myself going down rabbit holes with the chat and needing to start a new session so it didn't have any prior context. But then I lose everything that already existed before which could contain some useful snippets to look back at. Would be nice to have a some shortcut to say "ignore all prior context".
- Configure autocomplete by language. I have not looked to see if this exists but I would like to not have autocomplete in rust files but still have it in typescript until the model gets better
- UI generation. Something like vercel's v0.dev. This is a big feature but would be nice to iterate on UI ideas in my editor and get React components out of it.
- Allow selection of the CMD+I model. It looks like it uses the chat model but I found the output was often not good for inserting code. or perhaps default it to the tab autocomplete model?
- Better defaults. There are a lot of configuration options documented, but I think it is pretty unlikely that I would change them. Especially the completion options. It would be nice to have confidence that the defaults are optimized for my model (maybe they are but it isn't clear to me).
