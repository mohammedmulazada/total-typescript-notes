## Problem
A component (most likely from a library) does not export it's types.

## Solution

Use the [React Component props](https://react-typescript-cheatsheet.netlify.app/docs/advanced/patterns_by_usecase/#props-extracting-prop-types-of-a-component) to create a type that can get those types.

## Example

const Modal = (props: {title: string; visible: boolean; onClick: () => {} })

Type ModalProps = React.ComponentProps<typeof Modal>