JupiterNebula.Wyam.Shortcodes.TabBlock
======================================
[![Nuget](https://img.shields.io/nuget/v/JupiterNebula.Wyam.Shortcodes.TabBlock.svg)](https://www.nuget.org/packages/JupiterNebula.Wyam.Shortcodes.TabBlock/)

TabBlock provides a shortcode for [Wyam](https://wyam.io)'s Markdown module
to render Bootstrap-compatible tabs and tab panes in your Markdown pages.

![Output Demonstration](/assets/TabBlock/demo.gif)

## Adding TabBlock to Your Project

The following needs to be added to your config.wyam file:
```csharp
#n JupiterNebula.Wyam.Shortcodes.TabBlock
```

That's it! Wyam will discover the shortcode and automatically use it
when running the Markdown module.

## Configuration

Currently, TabBlock just outputs basic, Bootstrap-compatible tabs and
tab panes. Additional customization _is_ planned in the future though, so stay tuned!

## Usage

### Syntax

TabBlock uses the Wyam shortcode syntax, as defined in 
[Wyam's documentation](https://wyam.io/docs/concepts/shortcodes).

The easiest way to define tabs and tab pane content is by creating an unordered
list in Markdown syntax between the opening and closing tag for the TabBlock
shortcode as shown below:

```markdown
<?# TabBlock ?>
- ::Tab Label 1::
  All content here will be placed in the tab pane for this tab.
- ::Tab _Label_ 2 with ___formatting___::
  Any _valid_ [Markdown](https://daringfireball.net/projects/markdown/syntax) can
  be put __here__ as long as each line is indented so as to be included in
  the Markdown list item.
- ![Labels Can Be Images](https://jupiternebula.com/favicon.png){style="max-height: 1em;"}
  # Tab Pane Header
  Lorem ipsum...
<?#/ TabBlock ?>
```

Each tab above is represented as a Markdown list item. The
labels in this example come at the beginning of each item, wrapped in a
[MarkDig custom container](https://github.com/lunet-io/markdig/blob/master/src/Markdig.Tests/Specs/CustomContainerSpecs.md).
It is not required to wrap your tab labels, but it is good practice to do so.
This is because TabBlock receives your content after it has already been rendered into HTML and
interprets the first [HTML node](https://softwareengineering.stackexchange.com/a/264481)
it encounters in each list item as the label. The remaining nodes are interpretted as
the tab pane's content (what is shown to the user when the tab is selected).

As such, wrapping the label's text (in a `<span>` in the example above) makes sure the label
doesn't get mixed up with the tab pane's content and that it doesn't get cut off if
you add formatting to your label (such as the second tab above). The third tab's label
didn't need to be wrapped because the first node in that case is an `<a>` element and not just text.
Likewise, if your tab pane's content starts with an HTML element rather than just text, you
can safely use tab labels without having to wrap them (as you will see in the 
[examples](#Examples) section of this document).

### Examples

#### FizzBuzz in C#, F#, and Rust

Including code snippets in multiple languages is pretty common on software development blogs
and documentation pages. The following example presents implementations of FizzBuzz
from [rosettacode.org](https://rosettacode.org/wiki/FizzBuzz) for multiple languages
as separate tabs. 
````markdown
<?# TabBlock ?>
- C++
  ```cpp
  #include <iostream>
 
  int main ()
  {
      for (int i = 1; i <= 100; i++) 
      {
          if ((i % 15) == 0)
              std::cout << "FizzBuzz\n";
          else if ((i % 3) == 0)
              std::cout << "Fizz\n";
          else if ((i % 5) == 0)
              std::cout << "Buzz\n";
          else
              std::cout << i << "\n";
      }

      return 0;
  }
  ```
- F#
  ```fsharp
  let fizzbuzz n =
    match n%3 = 0, n%5 = 0 with
    | true, false -> "fizz"
    | false, true -> "buzz"
    | true, true  -> "fizzbuzz"
    | _ -> string n
 
  let printFizzbuzz() =
    [1..100] |> List.iter (fizzbuzz >> printfn "%s")
  ```
- Rust
  ```rust
  use std::borrow::Cow; // Allows us to avoid unnecessary allocations
  fn main() {
      (1..101).map(|n| match (n % 3, n % 5) {
          (0, 0) => "FizzBuzz".into(),
          (0, _) => "Fizz".into(),
          (_, 0) => "Buzz".into(),
          _ => Cow::from(n.to_string())
      }).for_each(|n| println!("{}", n));
  }
  ```
<?#/ TabBlock ?>
````
