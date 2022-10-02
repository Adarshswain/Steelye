# Steelye
Frontend Assignment

1. Explain what the simple List component does

Ans.List is generally used to store data in ordered format, here also this List is being used to render data in ordered format
list items are wrapped under WrappedSingleListItem.they are memoized under memo(WrappedListComponent) which basically
remembers all its components and all components which are not changed are not re - rendered on reloading our
app this increases speed and efficiency of our code

ex: - const elements = ['white', 'blue ', 'red' , 'green '];

const App = () => (
  <ul>
    {elements.map(item => {
      return <li key={item}>{item}</li>;
    })}
  </ul>
);

2. What problems / warnings are there with code

a. onClick events must have a function's reference instead of a function call.

  < li style = {{ backgroundColor: isSelected ? "green" : "red" }}
onClick = { onClickHandler(index) } >
  { text }
</li >
  Syntatical error in returned value of useState()
const [setSelectedIndex, selectedIndex] = useState();


b. Passing incorrect propType value in isSelected.Proptype defined is bool but here passed proptype is number
  < ul style = {{ textAlign: "left" }}>
  {
    items.map((item, index) => (
      <SingleListItem
        onClickHandler={() => handleClick(index)}
        text={item.text}
        index={index}
        isSelected={selectedIndex}
      />
    ))
  }
</ul >

c. items Array is null and iterating over null array is not possible or may cause run - time error
WrappedListComponent.defaultProps = {
  items: null,
};
Syntatical error in following code
WrappedListComponent.propTypes = {
  items: PropTypes.array(
    PropTypes.shapeOf({
      text: PropTypes.string.isRequired,
    })
  ),
};

d. Key prop is not present for uniquely identify each element

  < ul style = {{ textAlign: 'left' }}>
  {
    items.map((item, index) => (
      <SingleListItem
        onClickHandler={() => handleClick(index)}
        text={item.text}
        index={index}
        isSelected={selectedIndex}
      />
    ))
  }
</ul >

3. Please fix, optimize, and / or modify the component as much as you think is necessary

import React, { useState, useEffect, memo } from "react";
import PropTypes from "prop-types";

// Single List Item
const WrappedSingleListItem = ({ index, isSelected, onClickHandler, text }) => {
  return (
    <li
      style={{ backgroundColor: isSelected ? "green" : "red" }}
      onClick={() => onClickHandler(index)}
    >
      {text}
    </li>
  );
};

WrappedSingleListItem.propTypes = {
  index: PropTypes.number,
  isSelected: PropTypes.bool,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired,
};

const SingleListItem = memo(WrappedSingleListItem);

// List Component
const WrappedListComponent = ({ items }) => {
  const [selectedIndex, setSelectedIndex] = useState();

  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);

  const handleClick = (index) => {
    setSelectedIndex(index);
  };

  return (
    <ul style={{ textAlign: "left" }}>
      {items.map((item, index) => (
        <SingleListItem
          key={index}
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex === index}
        />
      ))}
    </ul>
  );
};

WrappedListComponent.propTypes = {
  items: PropTypes.arrayOf(
    PropTypes.shape({
      text: PropTypes.string.isRequired,
    })
  ),
};

WrappedListComponent.defaultProps = {
  items: [
    { text: "SteelEye", index: 1 },
    { text: "Adarsa", index: 2 },
  ],
};

const List = memo(WrappedListComponent);

export default List;

  ![9d7b0181-67ca-4aef-b2ff-4af3c970b7f4](https://user-images.githubusercontent.com/61945619/193465932-8241d835-d5df-4984-92f0-aa415ffe3b8d.jpg)

  
