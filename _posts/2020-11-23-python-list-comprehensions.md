---
layout: post
title: "Python List Comprehensions"
date: "2020-11-23 09:00:00 -0500"
tags: TIL Python
---

Python List Comprehensions are p _rad_. They go like this:

```python
variable = [out_exp for out_exp in input_list if out_exp == 2]
```

Here's a fun example taken from Exercism.io's Python track:

```python
number = 105
rain_drops = {3: "Pling", 5: "Plang", 7: "Plong"}
rain_sounds = [sound for value, sound in raindrops.items() if number % value == 0]
# rain_sounds: "PlingPlangPlong"
```
