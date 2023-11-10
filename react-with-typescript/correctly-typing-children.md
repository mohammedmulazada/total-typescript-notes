## Problem
The children prop of a component needs to be typed correctly.

## Solution

Use the `ReactNode` type, as it covers elements but also other things such as strings or undefined.

## Example

```ts
type Props = {
    children: ReactNode;
};

const Component = (props: Props) => {
    return <div>{props.children}</div>
}
```