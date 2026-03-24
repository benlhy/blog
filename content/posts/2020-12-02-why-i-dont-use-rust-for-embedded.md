---
title: "Opinion: Why I don't use Rust for embedded"
date: 2020-12-02T00:42:00+00:00
slug: "why-i-dont-use-rust-for-embedded"
draft: false
description: "Unfortunately, Rust for embedded development is still a little too new for me right now. C and C++ still hold the crown for embedded simply because of legacy and future support."
cover:
  image: "/images/2020/11/rustacean-flat-happy.png"
  relative: false
  hiddenInList: false
tags: ["opinion", "Rust", "2020", "Microcontroller", "STM32", "Silicon Labs", "Bluetooth", "C++", "C"]
---

Yet.

Rust is amazing. It has types, the compiler is robust and provides actual useful error reports instead of freaking out when I forget a `;` somewhere. You completely sidestep security problems that you commonly find in C simply because of how Rust is designed.

However, as much as I would love to use Rust as my tool for everything, the workflow for embedded devices still needs work.

- **Limited support for Rust** - This is the biggest stopper for me I think. Embedded development is firmly entrenched in C/C++ and most companies have no active plans to transition. If you want to work with a particular chip, you either have to write the crate yourself or rely on someone having already written the crate for it. This is okay if you're building stuff with popular ICs like the STM32 but the moment you venture out into the world of wireless or a slightly lesser known IC and you are on your own. As far as I can tell at the time of this writing, there are no embedded Bluetooth stacks in Rust, which is a big bummer for me.
- **Workflow** - This is down to personal taste, but I prefer to be debugging in an IDE where everything has been set up for me than to set up and customize a gdb server and workflow. For some people, there is value to setting up a workflow, but for me, I rather spend that time writing embedded code. This is relevant because most IDEs don't support development in Rust.
- **Vs C/C++** - I think there is more mileage to be had if you developed in C and C++ than in Rust, for the reasons stated above. Outside of embedded development, there is a whole host of projects that are written in C/C++. So it makes sense (to me at least) to gain proficiency in C than in Rust when doing embedded development.

For me at least, until we start seeing official support from manufacturers on Rust, I think I'll stick to using C and C++ for embedded development.
