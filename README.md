# Vaibhav-Kumar-Pandey_FrontEnd

Q.1 : Explain what the simple List component does.

Sol : After analysing the given code, according to my understanding of the List component  - 

-  The code starts with a functional component named as WrappedListComponent.

-  The List component accepts a prop called "items", which is an array of objects representing the items to be displayed. Each item object has a "text" property, which     is a string representing the text to be displayed for that item.

-  The List component uses the "useState" and "useEffect" hooks from React to manage the state of the selected item. When an item is clicked, the handleClick function    is called, which sets the "selectedIndex" state to the index of the clicked item.

-  Finally, the List component returns an unordered list element that contains multiple SingleListItem components. The "items" prop is mapped over to create one       "SingleListItem" component for each item in the list. Each SingleListItem component is passed as props such as "onClickHandler", "text", "index", and "isSelected"  based on the corresponding item object.

- Additionally, the SingleListItem and WrappedListComponent components are wrapped in the memo function to optimize performance by memoizing the components and avoiding unnecessary re-renders.

Q.2 : What problems / warnings are there with code?

Sol: The given code has few errors which needs to be fixed for smooth functioning of the code - 

1. In WrappedSingleListItem component, the onClickHandler should be passed as an arrow function  i.e.,  `onClick={onClickHandler(index)}` -> `onClick={() => onClickHandler(index)}`
2. In WrappedListComponent, there is a syntactical error in useState() i.e., `const [setSelectedIndex, selectedIndex] = useState();`  -> `const [selectedIndex, setSelectedIndex] = useState();`
3. In the given code snippet - arrayof should be used instead of array and shape should be used instead of shapeOf 
    
 WrappedListComponent.propTypes = {
          items: PropTypes.array(PropTypes.shapeOf({
            text: PropTypes.string.isRequired,
          })),
        };

4. In the WrappedListComponent, before iterating over the items we need to check if "items" has some elements and is not null and then we iterate using a map.
5. Also, while passing values to the props in SingleListItem Component we need to pass a bool value i.e, `isSelected={selectedIndex}` ->` isSelected={selectedIndex === index}`

Q.3 :  Please fix, optimize, and/or modify the component as much as you think is necessary.

Sol: Given below is the code with necessary optimizations and modifications - 

`import React, { useState, useEffect, memo } from 'react';
import PropTypes from 'prop-types';

// Single List Item
const WrappedSingleListItem = ({
  index,
  isSelected,
  onClickHandler,
  text,
}) => {
  return (
    <li
      style={{ backgroundColor: isSelected ? 'green' : 'red'}}
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
const WrappedListComponent = ({
  items,
}) => {
  const [selectedIndex, setSelectedIndex] = useState();

  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);

  const handleClick = index => {
    setSelectedIndex(index);
  };

  return (
    <ul style={{ textAlign: 'left' }}>
      {items && items.map((item, index) => (
        <SingleListItem
          key={index}
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex === index}
        />
      ))}
    </ul>
  )
};

WrappedListComponent.propTypes = {
  items: PropTypes.arrayOf(PropTypes.shape({
    text: PropTypes.string.isRequired,
  })),
};

WrappedListComponent.defaultProps = {
  items: null
};

const List = memo(WrappedListComponent);

export default List;`
