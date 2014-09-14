atomic-sass
===========
Atomic SASS is an atomic pattern bootstrap and a components sass system to build css for scalable website (including white marke) who love semantic markup.

Examples are build with BEM methodology but you can use whatever you want.

This project is inspired by the work of [Brad Frost](http://bradfrostweb.com/blog/post/atomic-web-design/), [Hugo Giraudel](http://hugogiraudel.com), [Robin Rendle](http://www.smashingmagazine.com/author/robin-rendle/?rel=author) and [DoCSSa](http://docssa.info) made by [Mathieu Larcher](http://www.ringabell.org) and [Fabien Zibi](http://fabienzibi.com).

## Atomic design
For more informations about this pattern you can check this links :
* http://bradfrostweb.com/blog/post/atomic-web-design/
* http://patternlab.io
* http://www.smashingmagazine.com/2013/08/02/other-interface-atomic-design-sass/

## Vocabulary
In this documentation I will use component to denote the atoms, molecules and organisms.


# How it works

## Tree structure
For this part I was inspired by Robin Rendel who write this great article http://www.smashingmagazine.com/2013/08/02/other-interface-atomic-design-sass/

I juste add like [DoCSSa](http://docssa.info), a sub part file starting with `__` who import any of sub file. For example `2_atoms/__atoms.scss` import all atoms. It's easy to spot and allows a lighter main.scss.

## Comment style
This project contain [styledocco](http://jacobrask.github.io/styledocco/) ready comments.

## Components
Define components with _mixin_ make your css more reusable. I recommande this article of [Hugo Giraudel - Bringing configuration to sass](http://hugogiraudel.com/2014/05/05/bringing-configuration-objects-to-sass/)

### Definition
I recommande to start with the _mixin_ :

    @mixin yourComponent($selector: '.yourComponent', $skin:'default'){
      ...
    }

Let's add some css rules in _placeholders_ :

    %yourComponent {
      display: inline-block;
    }

    %yourComponent__text {
      display: block;
    }

Extend them in your definition _mixin_ :

    @mixin yourComponent($selector: '.yourComponent', $skin:'default'){
      #{$selector} {
        @extend %yourComponent;

        &__text {
          @extend %yourComponent__text;
        }
      }
    }

### Placeholder skin system
For any component you can add a skin. It's usefull if you want to implement your component multiple time with variant, like a button for example. One by default with style on hover disable etc. And a second one with another color and an other function.
They can share some default style like `display: inline-block;` etc. But they have different color, spacing... So you put this differences in different skins.

To do this, you only have to do an other _mixin_ with the same selector skeleton. But instead of initial _placeholder_, suffix them with `-skin-#{$skin}`. With that you can interpolate and use any other _placeholder_ who finished with `-skin-youparam`. There is an example with default skin.

    %yourComponent-skin-default {
      background: #333;
    }

    %yourComponent__text-skin-default {
      color: #fff;
    }

    @mixin yourComponent-skin($selector, $skin) {
      #{$selector} {
        @extend %yourComponent-skin-#{$skin};

        &__text {
          @extend %yourComponent__text-skin-#{$skin};
        }
      }
    }

To finish, don't forget to include this skin _mixin_ in your master _mixin_:

    @mixin yourComponent($selector: '.yourComponent', $skin:'default'){
      @include yourComponent-skin($selector, $skin); // Don't forget to pass arguments
    }

There is a live example : <p class="sassmeister" data-gist-id="ab6a41a370c2ab201472" data-height="480"><a href="http://sassmeister.com/gist/ab6a41a370c2ab201472">Play with this gist on SassMeister.</a></p><script src="http://cdn.sassmeister.com/js/embed.js" async></script>
And the corresponding github gist : https://gist.github.com/hikkyu/ab6a41a370c2ab201472

### Declaration
Every component need to be declared with the `add-component` _mixin_. With that you register your component in the global variable `$components`. So you can extend any of them before expose.
It's very usefull for white mark system, on the one hand you have a _core_ how is standalone and on the other hand you can have a core-extend sass file to customize parameters and skin.

    @mixin add-component("component-name" [,"variationName"] [,option-map]);


### Expose _mixin_
Because we can not interpolate function and mixin name, the only solution I found is to expose your component with a special _mixin_:

    @mixin exposeYourComponent() {
      @each $declaration, $params in map-get($components, "yourComponent") {
        @include yourComponent($params...);
      }
    }

### Expose your component
Finally include exposeYourComponent mixin in `_component-expose.scss`


# Your in!
Feel free to send me your feedback, I will be happy to discuss ;-)
