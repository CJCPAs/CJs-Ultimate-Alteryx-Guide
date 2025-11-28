# Formula Function Reference

[Home](../../README.md) > [Resources](./README.md) > Formula Reference

---

Complete reference for Alteryx formula functions.

---

## Conditional Functions

### IF-THEN-ELSE
```
IF condition THEN result1 ELSE result2 ENDIF

IF condition1 THEN result1
ELSEIF condition2 THEN result2
ELSE result3
ENDIF
```

### IIF (Inline If)
```
IIF(condition, true_result, false_result)
```

### Switch
```
Switch(value, default, case1, result1, case2, result2, ...)
```

---

## Logical Functions

| Function | Description | Example |
|----------|-------------|---------|
| `AND` | Logical AND | `[A] > 0 AND [B] > 0` |
| `OR` | Logical OR | `[A] = 1 OR [B] = 1` |
| `NOT` | Logical NOT | `NOT [IsActive]` |
| `True()` | Returns True | `True()` |
| `False()` | Returns False | `False()` |

---

## Math Functions

### Basic
| Function | Description | Example |
|----------|-------------|---------|
| `+` | Addition | `[A] + [B]` |
| `-` | Subtraction | `[A] - [B]` |
| `*` | Multiplication | `[A] * [B]` |
| `/` | Division | `[A] / [B]` |
| `Mod(n, d)` | Remainder | `Mod(10, 3)` → 1 |
| `Pow(base, exp)` | Power | `Pow(2, 3)` → 8 |

### Rounding
| Function | Description | Example |
|----------|-------------|---------|
| `Abs(n)` | Absolute value | `Abs(-5)` → 5 |
| `Ceil(n)` | Round up | `Ceil(4.2)` → 5 |
| `Floor(n)` | Round down | `Floor(4.8)` → 4 |
| `Round(n, d)` | Round to d decimals | `Round(3.456, 2)` → 3.46 |

### Advanced
| Function | Description | Example |
|----------|-------------|---------|
| `Sqrt(n)` | Square root | `Sqrt(16)` → 4 |
| `Log(n)` | Natural log | `Log(10)` → 2.303 |
| `Log10(n)` | Log base 10 | `Log10(100)` → 2 |
| `Exp(n)` | e^n | `Exp(1)` → 2.718 |
| `Rand()` | Random 0-1 | `Rand()` → 0.534 |
| `RandInt(n)` | Random int 0 to n-1 | `RandInt(10)` → 7 |

### Statistical
| Function | Description |
|----------|-------------|
| `Average(n1, n2, ...)` | Average of values |
| `Max(n1, n2, ...)` | Maximum value |
| `Min(n1, n2, ...)` | Minimum value |

---

## String Functions

### Basic
| Function | Description | Example |
|----------|-------------|---------|
| `+` | Concatenate | `"Hello " + "World"` |
| `Length(s)` | String length | `Length("Hello")` → 5 |

### Case
| Function | Description | Example |
|----------|-------------|---------|
| `Uppercase(s)` | To uppercase | `Uppercase("hello")` → "HELLO" |
| `Lowercase(s)` | To lowercase | `Lowercase("HELLO")` → "hello" |
| `TitleCase(s)` | Title case | `TitleCase("john doe")` → "John Doe" |

### Extraction
| Function | Description | Example |
|----------|-------------|---------|
| `Left(s, n)` | First n chars | `Left("Hello", 2)` → "He" |
| `Right(s, n)` | Last n chars | `Right("Hello", 2)` → "lo" |
| `Substring(s, start, len)` | Extract portion | `Substring("Hello", 1, 3)` → "ell" |
| `GetWord(s, n)` | Get nth word | `GetWord("a b c", 1)` → "b" |

### Trimming
| Function | Description | Example |
|----------|-------------|---------|
| `Trim(s)` | Trim both ends | `Trim("  hi  ")` → "hi" |
| `TrimLeft(s)` | Trim left | `TrimLeft("  hi")` → "hi" |
| `TrimRight(s)` | Trim right | `TrimRight("hi  ")` → "hi" |

### Search/Replace
| Function | Description | Example |
|----------|-------------|---------|
| `FindString(s, find)` | Position of find (-1 if not found) | `FindString("Hello", "ll")` → 2 |
| `Replace(s, old, new)` | Replace text | `Replace("Hello", "l", "x")` → "Hexxo" |
| `ReplaceFirst(s, old, new)` | Replace first occurrence | |
| `Contains(s, find)` | True if contains | `Contains("Hello", "ell")` → True |
| `StartsWith(s, prefix)` | True if starts with | `StartsWith("Hello", "He")` → True |
| `EndsWith(s, suffix)` | True if ends with | `EndsWith("Hello", "lo")` → True |

### Padding
| Function | Description | Example |
|----------|-------------|---------|
| `PadLeft(s, n, char)` | Pad left to n chars | `PadLeft("5", 3, "0")` → "005" |
| `PadRight(s, n, char)` | Pad right to n chars | `PadRight("5", 3, "0")` → "500" |

### Splitting
| Function | Description |
|----------|-------------|
| `CountWords(s)` | Count words |
| `GetWord(s, n)` | Get nth word (0-based) |

---

## Date Functions

### Current Date/Time
| Function | Returns |
|----------|---------|
| `DateTimeNow()` | Current date and time |
| `DateTimeToday()` | Current date (no time) |

### Extract Components
| Function | Returns | Range |
|----------|---------|-------|
| `DateTimeYear(dt)` | Year | 4 digits |
| `DateTimeMonth(dt)` | Month | 1-12 |
| `DateTimeDay(dt)` | Day | 1-31 |
| `DateTimeHour(dt)` | Hour | 0-23 |
| `DateTimeMinute(dt)` | Minute | 0-59 |
| `DateTimeSecond(dt)` | Second | 0-59 |
| `DateTimeDayOfWeek(dt)` | Day of week | 1=Sun, 7=Sat |

### Date Arithmetic
```
DateTimeAdd(dt, n, units)
// units: "years", "months", "days", "hours", "minutes", "seconds"

DateTimeDiff(dt1, dt2, units)
// Returns number of units between dates
```

### Parsing and Formatting
```
DateTimeParse(string, format)
DateTimeFormat(datetime, format)
```

### Format Codes
| Code | Meaning | Example |
|------|---------|---------|
| `%Y` | 4-digit year | 2024 |
| `%y` | 2-digit year | 24 |
| `%m` | Month (01-12) | 01 |
| `%d` | Day (01-31) | 15 |
| `%H` | Hour 24h (00-23) | 14 |
| `%I` | Hour 12h (01-12) | 02 |
| `%M` | Minute (00-59) | 30 |
| `%S` | Second (00-59) | 45 |
| `%p` | AM/PM | PM |
| `%b` | Month abbrev | Jan |
| `%B` | Month full | January |
| `%a` | Day abbrev | Mon |
| `%A` | Day full | Monday |

---

## Conversion Functions

| Function | Description | Example |
|----------|-------------|---------|
| `ToNumber(s)` | String to number | `ToNumber("123")` → 123 |
| `ToString(n)` | Number to string | `ToString(123)` → "123" |
| `ToString(n, d)` | With decimals | `ToString(3.14, 2)` → "3.14" |
| `ToDate(s)` | String to date | `ToDate("2024-01-15")` |

---

## Null Functions

| Function | Description | Example |
|----------|-------------|---------|
| `IsNull(x)` | True if null | `IsNull([Field])` |
| `IsEmpty(x)` | True if null or empty | `IsEmpty([Field])` |
| `NULL()` | Returns null | `NULL()` |

---

## Regular Expression Functions

| Function | Description |
|----------|-------------|
| `REGEX_Match(s, pattern)` | True if pattern matches |
| `REGEX_Replace(s, pattern, replace)` | Replace matches |
| `REGEX_CountMatches(s, pattern)` | Count matches |

### Common Patterns
| Pattern | Matches |
|---------|---------|
| `\d` | Any digit |
| `\w` | Word character |
| `\s` | Whitespace |
| `.` | Any character |
| `*` | Zero or more |
| `+` | One or more |
| `?` | Zero or one |
| `^` | Start of string |
| `$` | End of string |
| `[abc]` | Any of a, b, c |
| `[^abc]` | Not a, b, c |

---

## Examples

### Safe Division
```
IIF([Denominator] = 0, 0, [Numerator] / [Denominator])
```

### Full Name
```
Trim([First]) + " " + Trim([Last])
```

### Age Calculation
```
DateTimeDiff(DateTimeToday(), [BirthDate], "years")
```

### Phone Formatting
```
"(" + Left([Phone], 3) + ") " +
Substring([Phone], 3, 3) + "-" +
Right([Phone], 4)
```

### Email Validation
```
REGEX_Match([Email], "^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}$")
```

---

[← Keyboard Shortcuts](02-Shortcuts.md) | [Next: External Resources →](04-External-Resources.md)
