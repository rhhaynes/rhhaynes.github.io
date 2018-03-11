---
layout: post
title:      "Fun with Hash Defaults"
date:       2018-03-11 17:43:42 +0000
permalink:  fun_with_hash_defaults
---

For this blog post I've decided to demonstrate some cool tricks I've either used or seen online regarding hash default values.  In all cases a default value is defined by either passing an argument (example 1) or code block (examples 2 and 3) to the Hash contructor.  As you'll see, changing the default value to something other than ```nil``` can enable some very interesting and powerful tools.

### 1. Create histograms
```
words_histogram = Hash.new(0)
=> {}

sentence = "How much wood could a woodchuck chuck if a woodchuck could chuck wood?"
=> "How much wood could a woodchuck chuck if a woodchuck could chuck wood?"

words = sentence.gsub(/[,.?!]/,"").split(" ").each do |word|
   words_histogram[word.downcase] += 1
end
=> ["How", "much", "wood", "could", "a", "woodchuck", "chuck", "if", "a", "woodchuck", "could", "chuck", "wood"]

words_histogram
=> {"how"=>1, "much"=>1, "wood"=>2, "could"=>2, "a"=>2, "woodchuck"=>2, "chuck"=>2, "if"=>1}
```

For reference some one-liners that count duplicate array elements are shown below.
```
(1) histogram = array.uniq.map{|elem| [elem, array.count(elem)]}.to_h
(2) histogram = array.each_with_object(Hash.new(0)){|elem,h| h[elem] += 1}
```

### 2. Generate lookup tables
```
square_roots = Hash.new do |h,k|
   h[k] = Math.sqrt(k)
end
=> {}

square_roots[2]
=> 1.4142135623730951

square_roots[4]
=> 2.0

square_roots[9]
=> 3.0

square_roots
=> {2=>1.4142135623730951, 4=>2.0, 9=>3.0}
```

### 3. Cache recursive functions
```
factorial = Hash.new do |h,k|
   k > 1  ?  h[k] = h[k-1] * k  :  h[k] = 1
end
=> {}

factorial[6]
=> 720

factorial
=> {1=>1, 2=>2, 3=>6, 4=>24, 5=>120, 6=>720}
```
