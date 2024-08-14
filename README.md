# Unity Convention Handbook

## 1. Commit message format
- Commit titles should start with a capital letter and be strictly one sentence long (no fullstop is needed).
- Commit messages must be phrased in present tense.
- Additional content should be placed in the commit description.
- Identifiers and other names should be enclosed in backticks.

**Example(s):**

CORRECT:
```
Add `PlayerController` script

Complete basic 2D movement and jump.
```

WRONG:
```
Added PlayerController script.
```

---

## 2. Commit behavior, size and frequency
- All commits need to work without further modifications.
- When someone checkouts a commit (with a detached HEAD), the project should still compile and run mostly free of any problems (semantic or otherwise).
- Check the build before making any commits and pushes.
- Commits should be small in size and self-contained.
- Commits should as best as possible not do more than 1 "big" thing as once.

---

## 3. Code horizontal formatting and wrapping behavior
- A line should be strictly capped at a length of 80 (range: [0, 80]).
- Manually wrapping is to be done should a line need more than 80 characters, as such:
    - Operator to be left at the previous line.
    - Where `L` is the original line, `L + 1` should have a `+1` indent relative to `L`.
    - When more than 1 round of wrapping is needed, `L + 2` and beyond should keep the same indentation level as `L + 1`.

    **Example(s):**
    ```csharp
    SuperDuperLongFunctionCall(veryVeryVeryVerboseArg1,
        veryVeryVeryVerboseArg2, veryVeryVeryVerboseArg3,
        veryVeryVeryVerboseArg4);
    ```

---

## 4. Access modifiers
- Explicit access modifier for all declarations, except within interfaces.
- Unity message methods should always be private.
- Serialized fields should always be private.
- Should a public variable be needed, always make it a property.

**Example(s):**
```csharp
// WRONG
string _someName = "Name";

// WRONG
void Update()
{
    ...
}

// CORRECT
private void Update()
{
    ...
}
```

---

## 5. Comments format
- All comments to start with `//` (per line needed).
- Multi-line comments (`/*` and `*/`) should not be used.
- The comment must be preceded by strictly 1 space character.
- The first character after the space should be captialized should it be [a-z].
- Comment line horizontal length should be around the same as the target code block.
- If the comment references an identifier in the code surround the identifier in backticks.

**Example(s):**
```csharp
// WRONG
// This is a very long and detailed comment as to why I have a bool here
private bool _extraBool = false;

// CORRECT
// This is a very long and detailed
// comment as to why I have a bool here
private bool _extraBool = false;
```
```csharp
private void Foo()
{
    // WRONG
    // Call Bar to init ...

    // CORRECT
    // Call `Bar` to init ...
    Bar();
    ...
}
```

---

## 6. Identifiers (top rules takes priority over bottom ones)
- Source files and Assets: `PascalCase`.
- Game object names: `PascalCase`.
- Types: `PascalCase`.
- Private variables (including serialized ones): `_camelCase` (note the leading underscore).
- Functions, properties and public variables: `PascalCase`.
- All other identifiers: `camelCase`.

---

## 7. Usings
- Leave a line between using groups.
- `System` usings should be first.
- `Unity` usings should be next.
- `TMPro` usings belong together with `Unity`.
- Order by semantics, then by length.
- Superfluous `using` to be removed.

**Example(s):**
```csharp
using System;

using TMPro;
using UnityEngine;
using UnityEngine.UI;
```

---

## 8. Create Asset Menu attribute
- Fill in both menu name and file name.

**Example(s):**
```csharp
[CreateAssetMenu(menuName = "Scriptable Object/Entity/Creature",
    fileName = "New Creature")]
public class Creature : Entity
{
    ...
}
```

---

## 9. Braces format
- Braces should always be on its own new line, never omit it.

**Example(s):**
```csharp
// CORRECT
if (condition)
{
    ...
}

// WRONG
if (condition) ...

// WRONG
if (condition)
    ...
```

---

## 10. Attribute format
- All attributes to have their own line.
- Rule 3 still applies for attributes.
- Header attributes should have 1 empty line above and below it, except when the top or bottom is an opening or closing brace.

**Example(s):**
```csharp
public bool SomeOtherJunk = true;

[Header("Vitals Drain Rate")]

[SerializeField]
[Range(0f, 5f)]
private float _hungerDrainPerSec = 1f;
```

---

## 11. Method declaration order
- Methods to be declared in the following order:
    - Public.
    - Protected.
    - Private.

---

## 12. Variable declaration order
- Variables to be declared in the following order:
    - Public (should have 1 empty line above and below it, except when the top or bottom is an opening or closing brace).
    - Private and serialized (whole block should have 1 empty line above and below it, except when the top or bottom is an opening or closing brace).
    - Private and for internal use only (group all together with the whole group having 1 empty line above and below it, except when the top or bottom is an opening or closing brace).

**Example(s):**
```csharp
public class PlayerVitals : MonoBehaviour
{
    public float StinkyFloat = 0f;

    [SerializeField]
    [Range(0f, 5f)]
    private float _hungerDrainPerSec = 1f;

    [SerializeField]
    [Range(0f, 5f)]
    private float _healthDrainPerSec = 0f;

    // Amount of health drain per second
    // when hunger is 0
    [SerializeField]
    [Range(0f, 10f)]
    private float _healthHungerZero = 5f;

    [SerializeField]
    private UICanvas.ActiveIn _updateActiveIn;

    private GameReset _gameReset;
    private UIManager _manager;

    private void SomeFunction()
    {
        ...
    }
}
```

---

## 13. Use asserts where appropriate to document assumed predicate(s)
- An assert message should be present for most of the cases.

**Example(s):**
```csharp
private void LoadKeyframes(List<int> indices)
{
    Assert.IsTrue(indices.Count > 0,
        "`indices` should not be empty as total " +
        "keyframes must be greater than 0");

    ...
}
```

---

## 14. Comment label format
- Use of labels such as `TODO`, `FIXME`, `SAFETY` and any others where appropriate.
- Format them as such `// NAME_OF_LABEL (NAME_OF_PERSON): Comment description`.

**Example(s):**
```
// TODO (Cheng Jun): Add descriptive comments here outlining how this works
// FIXME (Chris): A valid input is assumed here, add a validation check
// SAFETY (Brandon): The index is presumed to be in range, due to ...
```

---

## 15. Branch name and usage
- Branch names should only contain lowercase letters (numbers may be permitted where appropriate) and be delimited by a single dash (`-`).
- Do all development on the respective topic or feature branch, else commit to the `dev` branch.
- `main` branch should only contain "release ready" builds that have been sufficiently tested.

**Example(s):**
```
this-is-a-valid-branch-name
This-Is_not-a_valid_branch-name
```

---

## 16. All script components must be at the bottom
- All built-in and other components should be before script components.

---

## 17. Generic argument identifiers
- Prefer the use of semantic generic argument identifiers

**Example(s):**
```csharp
// WRONG
public class StateMachine<T, U>
{
    ...
}

// CORRECT
public class StateMachine<TSelfID, TStateID>
{
    ...
}
```

---

## 18. Type declarations
- Keep to 1 type declaration per source file.
- Nested types can be allowed depending on circumstance.
