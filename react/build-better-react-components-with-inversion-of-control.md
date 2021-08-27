---
date: 2021-08-18T22:25:49-04:00
title: Build Better React Components with Inversion of Control
description: Learn to use inversion of control to build flexible reusable React components. 
draft: false
categories: []
tags: []
toc: true

---

{{< demo url="https:://codesandbox.io/s/practical-sound-9ogdl" on="CodeSandbox" >}}

## Why Inversion of Control

Inversion of control gives consumers of a component more control over the styling 
and behavior of a component while abstracting away the complex parts of the
implementation. 

When building a reusable component it's difficult if not impossible to
anticipate all of the use cases ahead of time. Take a simple `Accordion`
component for example:

- Should the header have an icon?
- Should the icon be positioned to the left or the right of the text?
- Should each `Panel` open and close separately from the rest?
- When a `Panel` is toggled open should other `Panels`'s automatically close?
- Should one `Panel` always remain open?
- Should all `Panel`'s open and close at once?
- Should the `Panel`'s open to the bottom, top, left, or right?

It is possible to address all of these concerns by passing props into a
component. But this leads to an ever growing number of props and a great deal of
complexity in the implementation which makes it difficult to add new features without
breaking existing ones. The goal of inversion of control is to offload making these decisions 
to the consumers. This greatly simplifies the component and gives consumers 
the flexibility to handle a myriad of use cases while keeping the component 
implementation clean and light. 

## Customizable UI
In the example below the `Accordion` component gives consumers
complete freedom to customize the look of the `Accordion`. The text and icon (if
desired) may be positioned any which way without requiring changes to API to
support a new layout.

```tsx
<Accordion id={"BADA55"} type="uncontrolled">
  {({ isOpen, getToggleProps, getPanelProps }) => {
    return (
      <div>
        <Header {...getToggleProps()}>
          {textHeader}
          <Icon isOpen={isOpen} />
        </Header>
        <Panel {...getPanelProps()} style={getPanelStyles(isOpen)}>
          {textContents}
        </Panel>
      </div>
    );
  }}
</Accordion>
```

The ability to "lock down" the component is useful when there is a desire to
prevent developers from deviating from design guidelines. The example below 
demonstrates how a more specific implementation may be built from the previous
one. The use of specific `Header` and `Panel` components is required and it has more 
styling and behavior built in. This makes it easier to use, but less accommodating 
to serve as a component for multiple different designs.

```tsx
<StyledAccordion
  id={"BADA55"}
  type="uncontrolled"
>
  <StyledAccordion.Header>
    {({ isOpen }) => (
      <>
        {textHeader}
        <Icon isOpen={isOpen} />
      </>
    )}
  </StyledAccordion.Header>
  <StyledAccordion.Panel>{textContents}</StyledAccordion.Panel>
</StyledAccordion>
```

## Customizable Behavior

The example below demonstrates how a consumer can choose the desired
behavior for the `Accordion`. This is possible because the state is controlled by the
user and not the internals of the component.

- Should each `Panel` open and close separately from the rest?
- When a `Panel` is toggled open should other `Panels`'s automatically
close?
- Should one `Panel` always remain open?
- Should all `Panel`'s open and close at once?

```tsx
const [toggled, handleToggle] = React.useState("");

<Accordion
  id={"1"}
  type="controlled"
  onClick={() => handleToggle((cur) => (cur === "one" ? "" : "one"))}
  onKeyDown={() => {
    handleToggle((cur) => (cur === "one" ? "" : "one"));
  }}
  isOpen={toggled === "one"}
>
  {({ isOpen, getToggleProps, getPanelProps }) => {
    return (
      <div>
        <Header {...getToggleProps()}>
          {textHeader}
          <Icon isOpen={isOpen} />
        </Header>
        <Panel {...getPanelProps()} style={getPanelStyles(isOpen)}>
          {textContents}
        </Panel>
      </div>
    );
  }}
</Accordion>
<Accordion
  id={"2"}
  type="controlled"
  onClick={() => handleToggle((cur) => (cur === "two" ? "" : "two"))}
  onKeyDown={() => {
    handleToggle((cur) => (cur === "two" ? "" : "two"));
  }}
  isOpen={toggled === "two"}
>
  {({ isOpen, getToggleProps, getPanelProps }) => {
    return (
      <div>
        <Header {...getToggleProps()}>
          {textHeader}
          <Icon isOpen={isOpen} />
        </Header>
        <Panel {...getPanelProps()} style={getPanelStyles(isOpen)}>
          {textContents}
        </Panel>
      </div>
    );
  }}
</Accordion>
```

## Implementation

The Render Props pattern combined with Prop Getters is a great way to build inversion of control
into a component. In the example below the core logic is built into a hook separating it from the UI 
and making it easier to reuse. An `Accordion` component is then created from the hook using 
the Render Props pattern. Prop Getters encapsulate all of the details and can them selves 
be extended to receive props if the need arises `getPanelProps({...customOverrides})`. 

```tsx
interface GetPanelProps {
  toggleId: string;
  id: string;
  isOpen: boolean;
}

const getPanelProps = (props: GetPanelProps) => {
  const styles: React.CSSProperties = props.isOpen // hide text from screen readers
    ? { visibility: "visible" }
    : { visibility: "hidden" };

  return {
    id: props.id,
    "aria-labelledby": props.toggleId,
    style: styles
  };
};

interface EventProps {
  id: string;
  isOpen: boolean;
}

type OnClickToggle = (e: React.MouseEvent<HTMLElement>, x: EventProps) => void;
type OnKeyDownToggle = (
  x: EventProps,
  e: React.KeyboardEvent<HTMLElement>
) => void;

interface GetToggleProps {
  id: string;
  styles?: React.CSSProperties & { cursor: string };
  isOpen: boolean;
  onClick: OnClickToggle;
  onKeyDown: OnKeyDownToggle;
  panelId: string;
  disabled: boolean;
}

const getToggleProps = (props: GetToggleProps) => {
  const eventProps = { id: props.id, isOpen: props.isOpen };
  return {
    id: props.id,
    style: props.styles ?? { cursor: "pointer" },
    onClick: (e: React.MouseEvent<HTMLElement>) => props.onClick(e, eventProps),
    onKeyDown: (e: React.KeyboardEvent<HTMLDivElement>) => {
      if (e.key === " " || e.key === "Enter") {
        props.onKeyDown(eventProps, e);
      }
    },
    tabIndex: 0,
    role: "button",
    "aria-expanded": props.isOpen ? true : false,
    "aria-controls": props.panelId,
    "aria-disabled": props.disabled
  };
};

enum AccordionType {
  Controlled = "controlled",
  UnControlled = "uncontrolled"
}

interface BaseProps {
  initialOpen?: boolean;
  disabled?: boolean;
  id: string;
  type: string;
}

/**
 * @link https://www.w3.org/TR/wai-aria-practices/#accordion
 **/
export const useAccordion = (
  props:
    | (BaseProps & { type: "uncontrolled" })
    | (BaseProps & {
        isOpen: boolean;
        type: "controlled";
        onClick: OnClickToggle;
        onKeyDown: OnKeyDownToggle;
      })
) => {
  const [isOpen, setIsOpen] = React.useState(props.initialOpen ?? false);

  const toggleIsOpen = () => {
    if (props.disabled !== true) {
      setIsOpen((s) => !s);
    }
  };

  const toggleId = `${props.id}-toggle-button`;
  const panelId = `${props.id}-panel`;

  return {
    isOpen: props.type === AccordionType.Controlled ? props.isOpen : isOpen,
    getToggleProps: (props_?: Pick<GetToggleProps, "styles">) =>
      getToggleProps(
        props.type === AccordionType.Controlled
          ? {
              id: toggleId,
              panelId,
              styles: props_?.styles,
              onClick: props.onClick,
              isOpen: props.isOpen,
              onKeyDown: props.onKeyDown,
              disabled: false
            }
          : {
              isOpen,
              id: toggleId,
              panelId,
              styles: props_?.styles,
              onClick: toggleIsOpen,
              onKeyDown: (_, e) => {
                toggleIsOpen();
              },
              disabled: props.disabled ? true : false
            }
      ),
    getPanelProps: () =>
      getPanelProps(
        props.type === AccordionType.Controlled
          ? {
              isOpen: props.isOpen,
              id: panelId,
              toggleId
            }
          : {
              isOpen,
              id: panelId,
              toggleId
            }
      )
  };
};

export type AccordionChildProps = ReturnType<typeof useAccordion>;
export type AccordionParams = Parameters<typeof useAccordion>[0];

export const Accordion = (
  props: {
    children: (childProps: AccordionChildProps) => JSX.Element;
  } & AccordionParams
): JSX.Element => {
  const { children, ...rest } = props;
  const accordionProps = useAccordion(rest);

  return children(accordionProps);
};
```

## Locking Down the API

This example demonstrates one technique for creating components that fulfill
specific use cases. This is a great way to prevent unwanted styling changes and
keep the codebase DRY.

**Note**: This is demo quality code for a blog post.

```tsx
type Orientation = "left" | "right";

const isType = <A extends unknown, B extends unknown>(
  isT: (x: A | B) => boolean
) => (t: A | B): t is B => isT(t);

// eslint-disable-next-line @typescript-eslint/ban-types
const hasProp = (propName: string) => (x: {}) => propName in x;

const Container = styled.div<{ orientation?: Orientation }>`
  display: flex;
  flex-flow: ${(props) => (props.orientation ? "row" : "column")} nowrap;
  overflow: hidden;
`;

const Header = styled.div<{ isOpen?: boolean; orientation?: Orientation }>`
  display: flex;
  position: relative;
  font-size: 18;
  background: blueviolet;
  padding: 8px 16px;
  justify-content: space-between;
  flex-flow: row nowrap;
  z-index: 2;
  ${(props) =>
    props.isOpen
      ? css`
          box-shadow: 0px 3px 3px -3px;
        `
      : css``}
  &:focus, &:active {
    outline: 2px dashed black;
    overflow: visible;
    outline-offset: -2px;
  }
  ${(props) =>
    props.orientation
      ? `
        flex-flow: row nowrap;
        flex-basis: 600px;
        justify-content: space-between;
        align-items: center;
      `
      : ``}
`;

const Panel = styled.div(
  ({
    orientation,
    isOpen
  }: {
    orientation?: "left" | "right";
    isOpen: boolean;
  }) => {
    const cssY = isOpen
      ? css`
          transform: scaleY(1);
          transition: all 300ms;
          visibility: visible;
        `
      : css`
          transform: scaleY(0);
          height: 0;
          padding-top: 0;
          padding-bottom: 0;
          transition: all 300ms;
          visibility: hidden;
        `;

    const cssX = isOpen
      ? css`
          display: flex;
          flex-grow: 1;
          transition: all 300ms;
          transform: translateX(0%);
          visibility: visible;
        `
      : css`
          display: flex;
          flex-grow: 1;
          transition: all 300ms;
          visibility: hidden;
          transform: ${orientation === "right"
            ? "translateX(100%)"
            : "translateX(-100%)"};
        `;

    return css`
      font-size: 18;
      background: whitesmoke;
      padding: 8px 16px;
      justify-content: space-between;
      display: flex;
      flex-flow: row wrap;
      ${orientation ? cssX : cssY}
    `;
  }
);

interface ApiProps {
  orientation?: Orientation;
}

type ChildProps = AccordionChildProps &
  ApiProps &
  React.HTMLAttributes<HTMLDivElement>;

// eslint-disable-next-line @typescript-eslint/ban-types
type StyledAccordionChild<T extends {} = {}> = React.ReactElement<
  ChildProps & {
    children: React.ReactElement<T>;
  }
>;

interface StyledAccordionChildren {
  children: [StyledAccordionChild<{ isOpen: boolean }>, StyledAccordionChild];
}

export const StyledAccordion = (
  props: StyledAccordionChildren & AccordionParams & ApiProps
): JSX.Element => {
  const { children, ...rest } = props;
  const accordionProps = useAccordion(rest);
  const orderedChildren =
    props.orientation === "right" ? [children[1], children[0]] : children;

  return (
    <Container orientation={props.orientation}>
      {React.Children.map(orderedChildren, (child) =>
        React.cloneElement(child, {
          ...accordionProps,
          ...child.props,
          orientation: props.orientation
        })
      )}
    </Container>
  );
};

interface HeaderComponentChildren {
  children(props: { isOpen: boolean }): JSX.Element;
}

type HeaderComponentProps =
  | HeaderComponentChildren
  | (ChildProps & HeaderComponentChildren);

const isHeaderComponentProps = isType<
  HeaderComponentChildren,
  ChildProps & HeaderComponentChildren
>(hasProp("isOpen"));

const consoleErrorStyle = `background: deeppink; color: black; font-size: 1.5em; padding: 0.2em;`;

StyledAccordion.Header = function HeaderComponent(props: HeaderComponentProps) {
  if (!isHeaderComponentProps(props)) {
    console.log(
      `%c StyledAccordion.Header must be an immediate child of StyledAccordion.`,
      consoleErrorStyle
    );
    return null;
  }

  return (
    <Header
      isOpen={props.isOpen}
      orientation={props.orientation}
      {...props.getToggleProps()}
    >
      {props.children({ isOpen: props.isOpen })}
    </Header>
  );
};

interface PanelComponentChildren {
  children: JSX.Element | string;
}

type PanelComponentProps =
  | PanelComponentChildren
  | (ChildProps & PanelComponentChildren);

const isPanelComponentProps = isType<
  PanelComponentChildren,
  ChildProps & PanelComponentChildren
>(hasProp("isOpen"));

StyledAccordion.Panel = function PanelComponent(props: PanelComponentProps) {
  if (!isPanelComponentProps(props)) {
    console.log(
      "%c StyledAccordion.Panel must be an immediate child of StyledAccordion."
    );
    return null;
  }

  return (
    <Panel
      isOpen={props.isOpen}
      orientation={props.orientation}
      className={props.className}
      {...props.getPanelProps()}
    >
      {props.children}
    </Panel>
  );
};
```


