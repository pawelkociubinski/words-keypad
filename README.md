# Algorithms

Using the phone keypad, write a function that takes two parameters - a phone number and an array of words. 
Return a new array of words that contains only those words that can be created using the given phone number in the function's first argument. 


```typescript
const keypad = {
  1: [],
  2: ["a", "b", "c"],
  3: ["d", "e", "f"],
  4: ["g", "h", "i"],
  5: ["j", "k", "l"],
  6: ["m", "n", "o"],
  7: ["p", "q", "r", "s"],
  8: ["t", "u", "v"],
  9: ["w", "x", "y", "z"],
  0: []
} as const;

type keypadValues = typeof keypad[keyof typeof keypad];
type Letters = keypadValues[number];
type MappedLetters = { [key in Letters]: keyof typeof keypad };

const convertNumbersToLetters = () => {
  return Object.keys(keypad).reduce<MappedLetters>((accumulator, number) => {
    const value = keypad[number];

    return value.reduce((acc, letter) => {
      return { ...acc, [letter]: number };
    }, accumulator);
  }, {} as MappedLetters);
};

const letters = convertNumbersToLetters();

const convertWordToNumber = (splittedWord: string[]) => {
  return splittedWord.reduce((acc, letter) => {
    const number = letters[letter];

    return acc.concat(number);
  }, "");
};

const findWords = (phoneNumber: string, words: string[]) => {
  return words.reduce<string[]>((acc, word) => {
    const splittedWord = word.split("");
    const wordAsNumber = convertWordToNumber(splittedWord);

    if (phoneNumber.includes(wordAsNumber)) {
      return [...acc, word];
    } else {
      return acc;
    }
  }, []);
};

// input:
const phoneNumber = "3662277";
const words = ["foo", "bar", "baz", "foobar", "emo", "cap", "car", "cat"];

const result = findWords(phoneNumber, words);

// output:
console.log("result: ", result); // ["foo", "bar", "foobar", "emo", "cap", "car"]
```