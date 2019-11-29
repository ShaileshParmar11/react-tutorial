# react-tutorial
Easy way to have a personal **Guide + tutorial** of your react app.


## Install
```zsh
npm i -S react-tutorial
# or
yarn add react-tutorial
```

<small>[styled-components](https://www.styled-components.com/) it isn't bundled into the package and is required `styled-components@^4` and `react@^16.3` due to the use of [createRef](https://reactjs.org/docs/refs-and-the-dom.html#creating-refs), so: </small>

```zsh
npm i -S styled-components@^4.0.0
# or
yarn add styled-components@^4.0.0
```

## Usage

Add the `ReactTutorial` Component in your Application, passing the `steps` with the elements to highlight during the _ReactTutorial_.

```js
import React from 'react'
import ReactTutorial from 'react-tutorial'

class App extends Component {
  // ...

  render  (
    <>
      { /* other stuff */}
      <ReactTutorial
        steps={steps}
        playTour={this.state.playTour}
        onRequestClose={this.closeTour} />
    </>
  )
}

const steps = [
  {
    selector: '.first-step',
    content: 'This is my first Step',
  },
  // ...
]
```

### ReactTutorial Props

#### accentColor

> Change `--reactour-accent` _(defaults to accentColor on IE)_ css custom prop to apply color in _Helper_, number, dots, etc

Type: `string`

Default: `#007aff`

#### badgeContent

> Customize _Badge_ content using `current` and `total` steps values

Type: `func`

```js
// example
<ReactTutorial badgeContent={(curr, tot) => `${curr} of ${tot}`} />
```

#### children

> Content to be rendered inside the _Helper_

Type: `node | elem`

#### className

> Custom class name to add to the _Helper_

Type: `string`

#### disableDotsNavigation

> Disable interactivity with _Dots_ navigation in _Helper_

Type: `bool`

#### disableInteraction

> Disable the ability to click or intercat in any way with the _Highlighted_ element

Type: `bool`

#### disableKeyboardNavigation

> Disable all keyboard navigation (next and prev step) when true, disable only selected keys when array

Type: `bool | array(['esc', 'right', 'left'])`

```js
// example
<ReactTutorial disableKeyboardNavigation={['esc']} />
```

#### highlightedMaskClassName

> Custom class name to add to the element which is the overlay for the target element when `disableInteraction`

Type: `string`

#### inViewThreshold

> Tolerance in pixels to add when calculating if an element is outside viewport to scroll into view

Type: `number`

#### playTour

> You know…

Type: `bool`

Required: `true`

#### lastStepNextButton

> Change Next button in last step into a custom button to close the Tour

Type: `node`

```js
// example
<ReactTutorial lastStepNextButton={<MyButton>Done! Let's start playing</MyButton>} />
```

#### maskClassName

> Custom class name to add to the _Mask_

Type: `string`

#### maskSpace

> Extra Space between in pixels between Highlighted element and _Mask_

Type: `number`

Default: `10`

#### nextButton

> Renders as next button navigation

Type: `node`

#### onAfterOpen

> Do something after _Tour_ is opened

Type: `func`

```js
// example
<ReactTutorial onAfterOpen={target => (document.body.style.overflowY = 'hidden')} />
```

#### onBeforeClose

> Do something before _Tour_ is closed

Type: `func`

```js
// example
<ReactTutorial onBeforeClose={target => (document.body.style.overflowY = 'auto')} />
```

#### onRequestClose

> Function to close the _Tour_

Type: `func`

Required: `true`

#### prevButton

> Renders as prev button navigation

Type: `node`

#### rounded

> Beautify _Helper_ and _Mask_ with `border-radius` (in px)

Type: `number`

Default: `0`

#### scrollDuration

> Smooth scroll duration when positioning the target element (in ms)

Type: `number`

Default: `1`

#### scrollOffset

> Offset when positioning the target element after scroll to it

Type: `number`

Default: a calculation to the center of the viewport

#### showButtons

> Show/Hide _Helper_ Navigation buttons

Type: `bool`

Default: `true`

#### showNavigation

> Show/Hide _Helper_ Navigation Dots

Type: `bool`

Default: `true`

#### showNavigationNumber

> Show/Hide number when hovers on each Navigation Dot

Type: `bool`

Default: `true`

#### showNumber

> Show/Hide _Helper_ Number Badge

Type: `bool`

Default: `true`

#### startAt

> Starting step when _ReactTutorial_ is open the first time

Type: `number`

Default: `0`

#### stepWaitTimer

> delay of playing next step after current is completed

Type: `number`

Default: `0`

#### steps

> Array of elements to highligt with special info and props

Type: `shape`

Required: `true`

##### Steps shape

```js
steps: PropTypes.arrayOf(
        PropTypes.shape({
            selector: PropTypes.string,
            content: PropTypes.oneOfType([
                PropTypes.node,
                PropTypes.element,
                PropTypes.func,
              ]).isRequired,
            position: PropTypes.oneOfType([
                PropTypes.arrayOf(PropTypes.number),
                PropTypes.oneOf(['top', 'right', 'bottom', 'left', 'center']),
            ]),
            dropSelector: PropTypes.string,
            actionType: PropTypes.oneOf([1,2,3,4,5,6,10]),
            userTypeText: PropTypes.string,
            waitTimer: PropTypes.number,
            beforeStep: PropTypes.func,
            afterStep: PropTypes.func
        }),
```

##### actionType number

```js
CLICK: 1, // StepUp will be done when user click on element with given selector
DBL_CLICK: 2,  // StepUp will be done when user dbl click on element with given selector
TYPING: 3,  // StepUp will be done when user type exact string as userTypeText on element with given selector
DRAG_N_DROP: 4,  // StepUp will be done when user drag element with given selector and drop it to element with dropSelector
DRAG_WITH_MOUSE_MOVE:5,  // StepUp will be done when user perform mouseDown element with given selector, move it to dropSelector element and at last perform mouse up on the dropselector element
CUSTOM: 6,  // Control of stepUp is in authors hand by providing function to content wich will have goto and step as nextstep number
WAIT: 10  // StepUp will be done after waitTimer is over ( waitTimer will be in ms)
```

##### beforeStep

> func will be called before performing step

Type: `func`

##### afterStep

> func will be called before performing step

Type: `func`

##### Steps example

```js
const steps = [
  {
    selector: '[data-tour="my-first-step"]',
    content: ({ goTo, inDOM }) => (
      <div>
        Lorem ipsum <button onClick={() => goTo(4)}>Go to Step 5</button>
        <br />
        {inDOM && '🎉 Look at your step!'}
      </div>
    ),
    position: 'top',
    // you could do something like:
    // position: [160, 250],
    style: {
      backgroundColor: '#bada55',
    },
    // Disable interaction for this specific step.
    // Could be enabled passing `true`
    // when `disableInteraction` prop is present in Tour
    stepInteraction: false,
  },
  // ...
]
```