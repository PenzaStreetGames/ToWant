# MetaStruct

Модуль для генерации кода на языке Python

Интереснее, чем писать программы, может быть только написание программ, пишущих другие программы

### Представление программы в программе

Допустим, мы хотим представить программу в виде абстрактного дерева, например

```python
for i in range(5):
    print(i ** 2)
```

Можно представить как:

```json
{
  "for": {
    "vars": [
      "i"
    ],
    "iterator": {
      "function_call": {
        "name": "range",
        "args": [
          5
        ]
      }
    },
    "body": [
      {
        "function_call": {
          "name": "print",
          "args": [
            {
              "power": {
                "left": {
                  "name": "i"
                },
                "right": {
                  "number": 2
                }
              }
            }
          ]
        }
      }
    ]
  }
}
```

Выглядит достаточно громоздко, но можно провести некоторые упрощения.
Создадим классы-оболочки для каждого из ключевых слов, используемых в данном коде:

```python
# for -> For
# i -> Name("i")
# print() -> Name("print")
# range(5) -> Call(Name("range"), Args(5))
# i ** 2 -> Pow(Name("i"), Num(2))

```

И проробуем это записать вот так

```python
myfor = For(
    vars = [Name("i")],
    iterator = Call(Name("range"), Args(5))
    body = [
        Call(Name("print"), Args(
            Pow(Name("i"), Num(2))
        ))
    ]
)
```

Тогда при вызове метода `to_text` можно получить исходный кусочек кода:

```python
myfor.to_text()
"""
for i in range(5):
    print(i ** 2)
"""
```

### Генерация однотипных программ

Казалось бы, зачем так усложнять такую простую запись кода, какая есть в питоне? Ответ: чтобы её менять, и менять 
автоматически.

Например, сделаем 5 разных циклов при помощи одного:
```python
for i in range(1, 6):
    myfor.iterator.call_args[0] = i * 2
    print(myfor.to_text())

"""
for i in range(2):
    print(i ** 2)
    
for i in range(4):
    print(i ** 2)
    
for i in range(6):
    print(i ** 2)
    
for i in range(8):
    print(i ** 2)
    
for i in range(10):
    print(i ** 2)
"""
```

Программы получаются немного однотипными, но зато их можно сделать очень много с небольшими усилиями.

Задавать ключевые слова вручную удобно, так как ощущается полный контроль над ситуацией: что видим, то и пишем.
Но могла бы быть уместна какая-нибудь прослойка пользовательского кода.

Например, цикл тройной вложенности:

```python
triple_cycle("i", "range(5)", "j", "range(10)", "k", "range(2)")

"""
for i in range(5):
    for j in range(10):
        for k in range(2):
            pass
"""
```

Для выражений необходимо написать парсер, чтобы уметь не только выдавать код по классам, но и классы по коду.

### Самодобавляющийся код

В теории и на практике возможно изменение текста файла с программой на лету. То есть, после выполнения блок кода

```python
print("***")

triple_cycle("i", "range(5)", "j", "range(10)", "k", "range(2)").build()

print("***")
```

Превратится в:

```python
print("***")

for i in range(5):
    for j in range(10):
        for k in range(2):
            pass

print("***")
```

Таким образом, появляется некий аналог шаблонизатора кода без всяких сторонних сред разработки. Достаточно просто 
исполнить файл, а код подставится сам.

### Если уж совсем

Можно написать программу, которая пишет саму себя

