# Material UI

## What?

It's a React component library inspired by Google Material Design, with a focus on being mobile friendly (notably having most of the components for an Android App).

It's limited in number of components but there is enough, they are well documented & it's easy to install

This make it a good choice for our project.

## How to install

```bash
// with npm
npm install @material-ui/core
npm install @material-ui/icons

// with yarn
yarn add @material-ui/core
yarn add @material-ui/icons
```

Icons are optional but fun to use and there is one for pretty much everything described on the wirefames

## Some useful components

- Tabs for the main menu/toolbar: https://material-ui.com/components/tabs/#tabs
- Icons for everything you want (including that bar): https://material-ui.com/components/material-icons/
- List for showing images in, euh, list: https://material-ui.com/components/lists/. Works great with icons or image, allow secondary text
- ImageList for quick layout: https://material-ui.com/components/image-list/
- Grid for general layouting when needed
- Fab for the "+" floating like button: https://material-ui.com/components/floating-action-button/
- Chips for tags: https://material-ui.com/components/chips/#chip

## Styling 

Material UI uses "CSS in JS" kind of way to style - ie, if you want to add some styling, for example to have a Fab button goes on the usual bottom/left floating position, you use:

```javascript
import { makeStyles } from "@material-ui/core/styles";
...

// Style I want for fab, can add more - it's just a JS object
const useStyles = makeStyles({
  fab: {
    position: "fixed",
    right: "10px",
    bottom: "10px",
  },
});

export default MyComponent(props) {
  const classes = useStyles();
  ...


  return (
    <Fab className={classes.fab} color="primary" aria-label="add">
        <AddIcon />
    </Fab>
  )
}
```
