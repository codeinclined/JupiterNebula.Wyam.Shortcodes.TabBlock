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

#### Markdown
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
#### Rendered HTML (formatted by VSCode)
```html
<div class="tab-block" id="TabBlock__PabWwGf5">
    <ul class="nav nav-tabs" role="tablist">
        <li class="nav-item"><a class="nav-link active" data-toggle="tab" aria-selected="true"
                aria-controls="TabBlock__PabWwGf5-0-pane" href="#TabBlock__PabWwGf5-0-pane"
                id="TabBlock__PabWwGf5-0-link"><span>Tab Label 1</span></a></li>
        <li class="nav-item"><a class="nav-link" data-toggle="tab" aria-selected="false"
                aria-controls="TabBlock__PabWwGf5-1-pane" href="#TabBlock__PabWwGf5-1-pane"
                id="TabBlock__PabWwGf5-1-link"><span>Tab <em>Label</em> 2 with
                    <em><strong>formatting</strong></em></span></a></li>
        <li class="nav-item"><a class="nav-link" data-toggle="tab" aria-selected="false"
                aria-controls="TabBlock__PabWwGf5-2-pane" href="#TabBlock__PabWwGf5-2-pane"
                id="TabBlock__PabWwGf5-2-link"><img src="https://jupiternebula.com/favicon.png" class="img-fluid"
                    style="max-height: 1em; padding: 0;" alt="Labels Can Be Images"></a></li>
    </ul>
    <div class="tab-content">
        <div class="tab-pane show active" role="tabpanel" aria-labelledby="TabBlock__PabWwGf5-0-link"
            id="TabBlock__PabWwGf5-0-pane">
            All content here will be placed in the tab pane for this tab.</div>
        <div class="tab-pane" role="tabpanel" aria-labelledby="TabBlock__PabWwGf5-1-link"
            id="TabBlock__PabWwGf5-1-pane">
            Any <em>valid</em><a href="https://daringfireball.net/projects/markdown/syntax">Markdown</a>
            can be put <strong>here</strong> as long as each line is indented so as to be included in
            the Markdown list item.</div>
        <div class="tab-pane" role="tabpanel" aria-labelledby="TabBlock__PabWwGf5-2-link"
            id="TabBlock__PabWwGf5-2-pane">
            <h1 id="tab-pane-header">Tab Pane Header</h1>
            Lorem ipsum...
        </div>
    </div>
</div>
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

## Examples

### FizzBuzz in C#, F#, and Rust

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
