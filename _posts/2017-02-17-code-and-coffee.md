---
layout: post
title: Code and Coffee Hustle
categories:
  - elixir
  - clojure
  - rust
  - coffee
---

I love coding in coffee shops. It brings a few of my favorite things in the world together into one place: code, coffee, community, and music. So, when I took a solo trip up to Brooklyn I decided I would tour a handful of the local favorite coffee shops and do a code kata at each stop. I only managed to get to 3 before the caffeine got to me but experience was great fun. I’d like to share it with you!

What is a code kata? It’s a concept taken directly from martial arts where you build a simple application or small function with slight variations to hone your skills as a programmer. There is a strong focus on repetition which builds "mental muscle memory." I often do a code kata every morning at work just to warm up for the day and I will try out new languages, libraries and code editors when doing the kata.

The kata I chose was simple and is a personal favorite, the Roman numeral converter. The principle of this kata is quite simple: take a decimal number and convert it to its roman numeral equivalent. At first blush, that sounds hard but it is actually quite simple once you get a hang of the concept. What is hard is doing it in languages you don’t use every day while being jacked up on caffeine!

### Cafe Beit (Language: Elixir, Drink: Cortado)

![Beit Storefront](/public/images/beit.png)

I first passed by this shop on my way to pick up lunch at [The Meatball Shop](http://www.themeatballshop.com/) just off Bedford Ave and decided to make it my first stop. I chose to pair a cortado with Elixir because they seemed to have a lot in common. A cortado is two parts milk, one part espresso and with Elixir you get the power of [Erlang](https://www.erlang.org/) with a beautiful syntax inspired by Ruby.

Elixir is a strongly typed language with immutable variables, meaning that once you assign a value to a variable, you cannot modify it. This encourages a strong functional pattern and heavy use of recursion, which can take some time to get used to. But, I tinker around a lot with Elixir, so this was a breeze. In 20 minutes, I was done with an implementation that utilizes the power of Elixir function overloading via differing arity and Elixir’s ‘when’ keyword. For more information on this technique and identifying functions in Elixir, [check the docs](http://elixir-lang.org/crash-course.html#identifying-functions). 👓

~~~elixir
defmodule Romanize do
  @map [
    { "D",  500 },
    { "CD", 400 },
    { "C",  100 },
    { "L",  50 },
    { "XL", 40 },
    { "X",  10 },
    { "IX", 9 },
    { "V",  5 },
    { "IV", 4 },
    { "I",  1 }
  ]

  def convert(decimal) do
    convert(decimal, @map)
  end

  defp convert(0, _numerals), do: ""

  defp convert(decimal, [{ roman, arabic } | tail ]) when decimal >= arabic  do
    roman <> convert(decimal - arabic, [{ roman, arabic } | tail])
  end

  defp convert(decimal, [{ _roman, arabic } | tail]) when decimal < arabic do
    convert(decimal, tail)
  end
end
~~~

### Blue Bottle (Language: Rust, Drink: Machiato with sparkling water)

![Blue Bottle Storefront](/public/images/blue-bottle.png)

Ready for a challenge, I jogged over to Blue Bottle coffee and cracked open [Rust](https://www.rust-lang.org/en-US/) for the first time ever! I went with a Macchiato (one part espresso, one part milk) and a glass of sparkling water for this kata because that’s what Rust feels like to me: lots of power with just enough boundaries in place to keep you safe. The glass of sparkling water pairs well with a strong espresso drink like this just as [Cargo](https://crates.io/), the de-facto build tool for Rust, serves as an excellent pairing to Rust. I could never imagine using Rust without Cargo!

With a simple `cargo init` I was up and running. Rust even ships with an excellent test library that you can take advantage of with via the `cargo test` command. This kata took me nearly an hour and lots of reading to get done but I finally got it working!


~~~rust
#[derive(PartialEq, Debug)]

pub struct Roman
{
    numeral: String,
}

impl<'a> PartialEq<Roman> for &'a str {
    fn eq(&self, roman: &Roman) -> bool {
        self == &roman.numeral
    }
    fn ne(&self, roman: &Roman) -> bool {
        self != &roman.numeral
    }
}

static ARABIC_ROMAN : [(i32, &'static str); 13] = [
    (1000, "M"),
    (900, "CM"),
    (500, "D"),
    (400, "CD"),
    (100, "C"),
    (90, "XC"),
    (50, "L"),
    (40, "XL"),
    (10, "X"),
    (9, "IX"),
    (5, "V"),
    (4, "IV"),
    (1, "I")
];

impl From<i32> for Roman {
    fn from(decimal: i32) -> Roman {
        let mut decimal = decimal;
        let mut roman_numeral = String::new();
        for&(divisor, roman_char) in ARABIC_ROMAN.into_iter() {
            while decimal >= divisor {
                decimal -= divisor;
                roman_numeral.push_str(roman_char);
            }
        }
        Roman { numeral: roman_numeral }
    }
}
~~~

### Toby’s Estate Coffee (Language: Clojure, Drink: Cappuccino)

![Toby's Estate Storefront](/public/images/tobys.png)

Amped up on coffee and the high that I got from managing to actually complete this kata in Rust, I walked about one block over to Toby’s Estate Coffee. Ready to give my brain a bit of a break, I went with [Clojure](https://clojure.org/) and a Cappuccino. Clojure because it is one of my favorite languages to do code katas in, and a Cappuccino because I knew I would be waiting around a lot for the JVM to start up! Shortly after receiving my drink, I fired up [Leiningen](https://leiningen.org/) and initialized a new Clojure project. Leiningen is the de-facto build tool for Clojure to how Cargo for Rust. Instead of using the Clojure compiler you can just do a `lein run` or `lein test` to execute your code.

Clojure isn’t [strongly typed](https://en.wikipedia.org/wiki/Strong_and_weak_typing), so I didn’t have to deal with the same type errors I was struggling with in Rust and Elixir. The [polish notation](https://en.wikipedia.org/wiki/Polish_notation) style of function calling that Clojure uses along with parenthesis everywhere (as per the Lisp standard) can warp your brain for a few minutes as you get into that "mode" of thinking. I’ve never done this specific kata in Clojure so it took me longer than expected to complete. However, I got it done in just under 30 minutes. I went with a solution that involved writing my own reducer to iterate through a list of numerals and using some basic math to build up by roman numeral string with the proper roman lexicons.

~~~clojure
(ns roman.core)

(def numerals
  `((500 "D")
    (400 "CD")
    (100 "C")
    (90  "XC")
    (50  "L")
    (40  "XL")
    (10  "X")
    (9   "IX")
    (5   "V")
    (4   "IV")
    (1   "I")))

(defn roman-reducer [decimal numerals]
  (if (empty? numerals) ()
    (let [[[value roman-char]] numerals count (int (/ decimal value))]
      (cons (list count roman-char)
            (roman-reducer (- decimal (* count value)) (rest numerals))))))

(defn romanize [decimal]
  (apply str
         (flatten
          (map (fn [[count roman-char]] (take count (repeat roman-char)))
               (roman-reducer decimal numerals)))))
~~~

At this point I was zoomin’ on coffee and having a borderline panic attack! Heart racing, I strolled back to the Airbnb where I typed up the outline of this blog post. Now, I wouldn't suggest drinking 3 espresso drinks back to back but the exercise of doing a simple code kata in 3 different languages was a great experience. It helped me gain context on the difference between these three programming languages (Elixir, Clojure, Rust) that I don’t use every day. You should try it out yourself sometime with a few languages you have been curious about for a while!
