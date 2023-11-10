## Problem
TypeScript doesn't heavily enforce if you pass props that are not in the type with the function callback from useState.

```ts
import { useState } from "react";

interface TagState {
  tagSelected: number | null;
  tags: { id: number; value: string }[];
}

export const Tags = () => {
  const [state, setState] = useState<TagState>({
    tags: [],
    tagSelected: null,
  });
  return (
    <div>
      {state.tags.map((tag) => {
        return (
          <button
            key={tag.id}
            onClick={() => {
              setState((currentState) => ({
                ...currentState,
                // @ts-expect-error
                tagselected: tag.id, // this should error but it does not
              }));
            }}
          >
            {tag.value}
          </button>
        );
      })}
      <button
        onClick={() => {
          setState((currentState) => ({
            ...currentState,
            tags: [
              ...currentState.tags,
              {
                id: new Date().getTime(),
                value: "New",
                // @ts-expect-error
                otherValue: "something", // this should error but it does not
              },
            ],
          }));
        }}
      >
        Add Tag
      </button>
    </div>
  );
};
```

## Solution

So it seems that when TypeScript compares objects, they have to match each other, but with functions that is not the case. It is possible to enforce this however.

## Example

```ts
type Example = {
    message: string
};

type FunctionReturnType = () => Example;

const firstObject: Example = {
    message: 'hi';
    extraField: 'wow'; // this will error
};

const secondObject: FunctionReturnType = () => ({
    message: 'hi';
    extraField: 'wow'; // this will not error
})

const thirdObject: FunctionReturnType = (): Example => ({
    message: 'hi';
    extraField: 'wow'; // this will error now
})
```

### To fix the above error

```ts
import { useState } from "react";

interface TagState {
  tagSelected: number | null;
  tags: { id: number; value: string }[];
}

export const Tags = () => {
  const [state, setState] = useState<TagState>({
    tags: [],
    tagSelected: null,
  });
  return (
    <div>
      {state.tags.map((tag) => {
        return (
          <button
            key={tag.id}
            onClick={() => {
              setState((currentState): TagState => ({
                ...currentState,
                // @ts-expect-error
                tagselected: tag.id,
              }));
            }}
          >
            {tag.value}
          </button>
        );
      })}
      <button
        onClick={() => {
          setState((currentState): TagState => ({
            ...currentState,
            tags: [
              ...currentState.tags,
              {
                id: new Date().getTime(),
                value: "New",
                // @ts-expect-error
                otherValue: "something",
              },
            ],
          }));
        }}
      >
        Add Tag
      </button>
    </div>
  );
};
```