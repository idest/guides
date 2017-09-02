### Avoid line breaks between divs when using inline-block and relative widths

#### Option 1: Remove whitespace from your html code

    <div class="containter">
      <div>
        ...
      </div><div>
        ...
      <div>
    </div>

Drawbacks: Your HTML layout won't be as pretty.

#### Option 2: Set the container div font-size to 0

Assuming the html structure from the previous example:

    .container {
      text-size: 0;
    }

Drawbacks: All child elements of the container div will have the text-size set
to 0, and if you have text you will have to manually specify its size.

#### Option 3: Set  the white_space property of the container div to nowrap

    .container {
       text-size: 0;
    }

Drawbacks: Whitespace will still exist, it will still cause lines to be wider than their container elements, it just won't generate line breaks.
