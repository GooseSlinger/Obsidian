## Примеры функций `inflect.engine()`

```python
import inflect
p = inflect.engine()

# 📌 Число в текст (number → words)
print(p.number_to_words(123))
# → 'one hundred and twenty-three'

# 📌 Множественное число слова (plural form)
print(p.plural("cat"))
# → 'cats'

# 📌 Согласование слова с числом (ед./мн. в зависимости от количества)
print(p.plural_noun("apple", 1))
# → 'apple'
print(p.plural_noun("apple", 2))
# → 'apples'

# 📌 Автоматический выбор артикля "a"/"an"
print(p.a("apple"))
# → 'an apple'
print(p.a("banana"))
# → 'a banana'

# 📌 Порядковая форма числа (1st, 2nd, 3rd...)
print(p.ordinal(3))
# → '3rd'

# 📌 Текстовая порядковая форма числа (first, second, third...)
print(p.ordinal_number(3))
# → 'third'
```

