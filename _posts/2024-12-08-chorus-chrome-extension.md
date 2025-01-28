---
layout: post
title:  "Chorus: A Chrome extension to compare LLMs"
date:   2025-01-28 00:00:00 -0800
---

I've written a little Chrome extension to compare responses from different LLMs in their native browser UI. You can try it out in the Chrome web store [here](https://chromewebstore.google.com/detail/chorus-compare-llms/opedkjbjehljdkjahbingfncglipdeif).

<!--more-->

I've found myself frequently copy-pasting queries between different chatbots. [Chorus](https://chromewebstore.google.com/detail/chorus-compare-llms/opedkjbjehljdkjahbingfncglipdeif) automates this. You can type `ch` `Space` in the omnibar, write your prompt, and hit `Enter`. Chorus will create a tab group and send your prompt to all the enabled models.

![Chorus demo](/assets/chorus.gif)

The extension currently supports Claude, ChatGPT, Gemini and DeepSeek, and also lets you send your query to some search engines (Google, Perplexity).

Beyond benchmarks, my best evaluation of LLMs is comparing responses on actual problems in my life. Rather than picking a favorite model and going to it with every question, I've been finding it valuable to try all the top models, especially when I have a real-world problem that's at the edge of current LLM capabilities. There's also an interesting "ensembling" effect, where the union of responses beats any individual response. This is especially true on open-ended questions, like seeking recommendations or advice.

It would be nice to load all the LLM responses side-by-side, but I didn't want to build a new UI that calls the LLM APIs. There's a lot of features that these chatbots only support in the browser UI, like rendering artifacts and tracking conversation history. Also, for the LLMs that support subscriptions, this lets you use your existing subscription rather than being charged on your API key.

I've found the omnibar-based workflow to be snappy for quick queries, but it's a bit clunky for longer queries, and doesn't support attaching images and files. I might add a separate page to the extension for this.

I also realized that none of the top LLM web apps properly support URL parameters, so I had to hack some content scripts that simulate typing and clicking on the page. It'd be great if the LLM providers could support something like `url/?q=prompt&model=name`.

The code lives in this [GitHub repo](https://github.com/rishicomplex/chorus). Feedback and contributions are welcome!