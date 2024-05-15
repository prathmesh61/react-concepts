# react-concepts

# Mastering S.O.L.I.D Principles in React: Easy Examples and Best Practices

# Single Responsibility Principle (SRP)

A component should have only one reason to change, meaning it should have only one job.
Example: User Profile Component Do:
Split responsibilities into smaller, functional components.

## Usage/Examples

```javascript
// UserProfile.js
const UserProfile = ({ user }) => {
  return (
    <div>
      <UserAvatar user={user} />
      <UserInfo user={user} />
    </div>
  );
};

// UserAvatar.js
const UserAvatar = ({ user }) => {
  return <img src={user.avatarUrl} alt={`${user.name}'s avatar`} />;
};

// UserInfo.js
const UserInfo = ({ user }) => {
  return (
    <div>
      <h1>{user.name}</h1>
      <p>{user.bio}</p>
    </div>
  );
};
```

# Don't:

Combine display, data fetching, and business logic in one component.

```javascript
// IncorrectUserProfile.js
const IncorrectUserProfile = ({ user }) => {
  // Fetching data, handling business logic and displaying all in one
  const handleEdit = () => {
    console.log("Edit user");
  };

  return (
    <div>
      <img src={user.avatarUrl} alt={`${user.name}'s avatar`} />
      <h1>{user.name}</h1>
      <p>{user.bio}</p>
      <button onClick={handleEdit}>Edit User</button>
    </div>
  );
};
```

# Open/Closed Principle (OCP)

Software entities should be open for extension, but closed for modification.

Example: Themable Button

# Do:

-- Use props to extend component functionalities without modifying the original component.

```javascript
// Button.js
const Button = ({ onClick, children, style }) => {
  return (
    <button onClick={onClick} style={style}>
      {children}
    </button>
  );
};

// Usage
const PrimaryButton = (props) => {
  const primaryStyle = { backgroundColor: "blue", color: "white" };
  return <Button {...props} style={primaryStyle} />;
};
```

# Don't:

Modify the original component code to add new styles or behaviors directly.

```javascript
// IncorrectButton.js
// Modifying the original Button component directly for a specific style
const Button = ({ onClick, children, primary }) => {
  const style = primary ? { backgroundColor: "blue", color: "white" } : null;
  return (
    <button onClick={onClick} style={style}>
      {children}
    </button>
  );
};
```

# Liskov Substitution Principle (LSP)

Objects of a superclass shall be replaceable with objects of its subclasses without breaking the application.

Example: Basic Button and Icon Button

# Do:

Ensure subclass components can replace superclass components seamlessly.

```javascript
// BasicButton.js
const BasicButton = ({ onClick, children }) => {
  return <button onClick={onClick}>{children}</button>;
};

// IconButton.js
const IconButton = ({ onClick, icon, children }) => {
  return (
    <button onClick={onClick}>
      <img src={icon} alt="icon" />
      {children}
    </button>
  );
};
```

# Don't:

Introduce subclass-specific properties that break functionality when substituting.

```javascript
// IncorrectIconButton.js
// This button expects an icon and does not handle the absence of one, breaking when used as a BasicButton
const IncorrectIconButton = ({ onClick, icon }) => {
  if (!icon) {
    throw new Error("Icon is required");
  }
  return (
    <button onClick={onClick}>
      <img src={icon} alt="icon" />
    </button>
  );
};
```

# Interface Segregation Principle (ISP)

No client should be forced to depend on methods it does not use.

Example: Text Component

# Do:

Provide specific interfaces for different uses.

```javascript
// Text.js
const Text = ({ type, children }) => {
  switch (type) {
    case "header":
      return <h1>{children}</h1>;
    case "title":
      return <h2>{children}</h2>;
    default:
      return <p>{children}</p>;
  }
};
```

# Don't:

Clutter a component with unnecessary properties.
// IncorrectText.js

```javascript
// IncorrectText.js
// This component expects multiple unrelated props, cluttering the interface
const IncorrectText = ({ type, children, onClick, isLoggedIn }) => {
  if (isLoggedIn && onClick) {
    return (
      <a href="#" onClick={onClick}>
        {children}
      </a>
    );
  }
  return type === "header" ? <h1>{children}</h1> : <p>{children}</p>;
};
```
