# TypeScript и инструменты разработчика Notes

## Important notes and links

---

---

## 3 Сборка

`npm i -D webpack webpack-cli` - установка webpack

## 4. Компиляция и примитивные типы

`tsc` - компилятор TS
`ts-node-dev` - инструмент для запуска кода в режиме разработки

## 8. Утилитарные типы

```typescript
Partial<T> // частичный generic  
Required<T>  
Readonly<T>  
Record<K, T>  
Pick<T, K>  
Omit<T, K>

Exclude<T, U>
Extract<T, U>
NonNullable<T>

ReturnType<T>
Parameters<T>
```

## 9.2 Введение в тестирование

Написание любого из видов тестов — это польза в долгосрочной перспективе,
заключающаяся в снижении количества багов, с которыми столкнётся пользователь.
