
```markdown
# UI/UX Practical Notes

This document contains step-by-step notes for UI/UX practicals performed using **Figma / FigJam**.

---

# Practical 3: User Journey Persona

## Conduct Basic User Research
Ask users questions about **hotel booking apps**.

Example questions:
- Age and occupation
- How often they use technology
- Which hotel booking apps they use
- Their goals when using the app
- Their frustrations (slow loading, hidden charges, etc.)

---

## Create 2 User Personas

Steps:

1. Open **FigJam**
2. Click **+ → Templates**
3. Search **User Persona**
4. Select the template
5. Edit details according to the **survey results**

Example fields to edit:
- Name
- Age
- Occupation
- Goals
- Frustrations
- Technology usage

---

# Practical 6: Vertical & Horizontal Scrolling in Figma

## Create Mobile Frame

1. Click **Frame Tool (F)**
2. Select **iPhone 14 / 390 × 844**
3. Rename frame → **Home Screen**

---

## Create Search Bar

1. Press **R → Rectangle**
2. Size:
   - Width: **350**
   - Height: **40**
3. Corner Radius → **20**

Add:
- Text → `Hinted search text`
- Menu icon (left)
- Search icon (right)

Place the search bar **at the top of the frame**.

---

## Create Horizontal Scrolling Food Cards

Steps:

1. Insert **4–6 circle images**
2. Paste images using:

```

Ctrl + Shift + V

```

3. Select all circles
4. Press:

```

Shift + A

```

5. Apply **Auto Layout → Horizontal**

Create a **frame** and drag the images inside the frame.

Go to **Prototype Panel**:

```

Overflow Behavior → Horizontal Scrolling

```

Now the row **scrolls left and right**.

---

## Create Vertical Scrolling Food Cards

These are the **large food images in the center of the screen**.

Steps:

1. Create rectangle

```

Width: 350
Height: 180

```

2. Add food image
3. Duplicate **3–4 times**

Select all cards:

```

Shift + A → Auto Layout

```

Create a **frame** and place them inside.

Set:

```

Direction → Vertical
Spacing → 20

```

Run **Prototype** to test scrolling.

---

# Practical 7: Website Redesign (Color & Typography using Variables)

## Create Desktop Frame

1. Press **F**
2. Select **Desktop 1440px**

---

## Create Color Variables

Do **not select any frame**.

Steps:

1. Open **Variables**
2. Create **Practice Collection**

Add variables:

| Variable Name | Color |
|---------------|------|
| Primary | #FF7A00 |
| Secondary | #222222 |

---

## Create Number Variables

Example:

| Variable | Value |
|---------|------|
| Size | 34 |
| Size | 10 |

These variables help maintain **consistent typography and spacing**.

---

# Practical 9: Gamification

Steps:

1. Create a **Frame → iPhone 17**
2. Create a **Rectangle**
3. Add text

Create **second frame**.

Go to:

```

Plugins → Widgets → Assets

```

Search:

```

LottieFiles

```

Steps:

1. Run plugin
2. Insert animation
3. Close plugin

Run **Prototype** to test animation.

---

# Practical 10: Micro Animation

## Progress Bar Animation

1. Create a **rectangle**
2. Create **second rectangle inside it**
3. Color → **Green**

Select both rectangles:

```

Create Component

```

Right panel:

```

Component → Properties → + Variant

```

Click **+** to create variants.

Go to **Prototype**:

```

Interaction → After Delay
Animation → Smart Animate

```

Create a **new frame**.

From **Assets → Components**, drag the **progress bar**.

---

## Loader Animation

Steps:

1. Create **multiple circles**
2. Select all circles
3. Convert to **Component**

Add **variants**:

```

* Variant (4 times)

```

Alignment:

```

Center Vertical
Center Horizontal

```

Create new **frame**.

From **Assets**, drag the **loader component**.

Add prototype interaction:

```

On Delay

```

---

## Status Bar Animation

Steps:

1. Create **circle**
2. Copy paste another circle
3. Set **Arc = 25%**

Create **Component → Variant**

Add more variants:

```

Arc 50%
Arc 75%
Arc 100%

```

Create a **frame** and place the component inside.

---

## Welcome Screen

Create a **simple welcome UI** with:

- Heading
- Illustration
- Button

Add **micro animation** for better user experience.

---

# Summary

These practicals demonstrate key **UI/UX design techniques**:

- User Research & Personas
- Vertical & Horizontal Scrolling
- Website Redesign with Variables
- Gamification Elements
- Micro Animations
```

