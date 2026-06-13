# JWT Authentication Notes (Express.js)

### Packages
```
npm install jsonwebtoken bcrypt cookie-parser
```
---

## Cookie Options

```

const cookieOptions = {
    httpOnly: true,
    secure: process.env.NODE_ENV === "production",
    sameSite: "strict",
    maxAge: 7 * 24 * 60 * 60 * 1000
};
```

---

## Signup Controller

```
const { name, email, password } = req.body;

const existingUser = await User.findOne({ email });

if (existingUser) {
    return res.status(400).json({
        success: false,
        message: "User already exists"
    });
}

const hashedPassword = await bcrypt.hash(password, 10);

const user = await User.create({
    name,
    email,
    password: hashedPassword
});

const token = jwt.sign(
    { id: user._id },
    process.env.JWT_SECRET,
    { expiresIn: "7d" }
);

res.cookie("token", token, cookieOptions);

res.status(201).json({
    success: true,
    user
});

Note: "jwt.sign()" basically generates a JWT token by taking a payload (e.g. "{ id: user._id }"), signing it with a secret key, and returning the token string. 

```
---
 
## Login Controller

```

const { email, password } = req.body;

const user = await User.findOne({ email });

if (!user) {
    return res.status(404).json({
        success: false,
        message: "User not found"
    });
}

const isMatch = await bcrypt.compare(
    password,
    user.password
);

if (!isMatch) {
    return res.status(401).json({
        success: false,
        message: "Invalid credentials"
    });
}

const token = jwt.sign(
    { id: user._id },
    process.env.JWT_SECRET,
    { expiresIn: "7d" }
);

res.cookie("token", token, cookieOptions);

res.status(200).json({
    success: true,
    message: "Login Successful"
});

---

Logout Controller

res.clearCookie("token");

res.status(200).json({
    success: true,
    message: "Logout Successful"
});

```
---

## Authentication Middleware

```
import jwt from "jsonwebtoken";

export const isAuthenticated = (
    req,
    res,
    next
) => {

    const token = req.cookies.token;

    if (!token) {
        return res.status(401).json({
            success: false,
            message: "Please login first"
        });
    }

    try {

        const decoded = jwt.verify(
            token,
            process.env.JWT_SECRET
        );

        req.user = decoded;

        next();

    } catch (error) {

        return res.status(401).json({
            success: false,
            message: "Invalid token"
        });
    }
};
```
---

## Authorization Middleware

```

export const authorizeRoles =
(...roles) => {

    return (req, res, next) => {

        if (
            !roles.includes(req.user.role)
        ) {
            return res.status(403).json({
                success: false,
                message: "Forbidden"
            });
        }

        next();
    };
};
```
---

Protected Route
```
router.get(
    "/profile",
    isAuthenticated,
    getProfile
);
```
---

Admin Route
```
router.get(
    "/admin",
    isAuthenticated,
    authorizeRoles("admin"),
    adminDashboard
);
```
---

 ## JWT Flow

```
Signup/Login
      ↓
jwt.sign() generates token
      ↓
Store in Cookie
      ↓
Browser sends Cookie
      ↓
Authentication Middleware
      ↓
jwt.verify()
      ↓
req.user
      ↓
Protected Route Access
```
---

### Authentication vs Authorization

1. Authentication:
- Who are you?

isAuthenticated

2. Authorization:
- What can you access?

authorizeRoles("admin")

---

### Status Codes
```
200 → Success

201 → Created

400 → Bad Request

401 → Unauthorized

403 → Forbidden

404 → Not Found

500 → Internal Server Error
```