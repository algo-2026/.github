# **Do/Don't (базовые правила с примерами)**

## **Do:**

1. **Используйте семантические имена для функций, переменных и структур.**
   **Плохо:**

   ```c
   int f(int a) { return a*2; }
   ```

   **Хорошо:**

   ```c
   int doubleValue(int number) { return number * 2; }
   ```

2. **Инициализируйте все переменные и структуры сразу после объявления.**
   **Плохо:**

   ```c
   struct Point p;
   p.x = 0;
   p.y = 0;
   ```

   **Хорошо:**

   ```c
   struct Point p = { .x = 0, .y = 0 };
   ```

3. **Минимизируйте область видимости переменных и функций (`static` для внутренних функций).**
   **Плохо:**

   ```c
   void helper() { ... } // видно везде
   ```

   **Хорошо:**

   ```c
   static void helper() { ... } // видно только в этом файле
   ```

4. **Проверяйте аргументы функций, особенно указатели и пользовательский ввод.**
   **Плохо:**

   ```c
   void printLength(char* str) { printf("%zu\n", strlen(str)); }
   ```

   **Хорошо:**

   ```c
   void printLength(const char* str) {
       if (!str) return;
       printf("%zu\n", strlen(str));
   }
   ```

5. **Разделяйте бизнес-логику и ввод/вывод; возвращайте результат через значения или структуры.**
   **Плохо:**

   ```c
   int sumAndPrint(int a, int b) {
       printf("%d\n", a+b);
       return a+b;
   }
   ```

   **Хорошо:**

   ```c
   int sum(int a, int b) {
       return a + b;
   }
   // вывод отдельно
   printf("%d\n", sum(2,3));
   ```

---

## **Don't:**

1. **Не используйте магические числа — создавайте константы или макросы.**
   **Плохо:**

   ```c
   int area = width * 3; // почему 3?
   ```

   **Хорошо:**

   ```c
   #define HEIGHT 3
   int area = width * HEIGHT;
   ```

2. **Не игнорируйте ошибки функций (например, проверку `malloc`, `fopen`).**
   **Плохо:**

   ```c
   char* buf = malloc(100);
   strcpy(buf, "hello");
   ```

   **Хорошо:**

   ```c
   char* buf = malloc(100);
   if (!buf) { perror("malloc failed"); exit(1); }
   strcpy(buf, "hello");
   ```

3. **Не используйте глобальные переменные без необходимости.**
   **Плохо:**

   ```c
   int counter;
   void inc() { counter++; }
   ```

   **Хорошо:**

   ```c
   void inc(int* counter) { (*counter)++; }
   ```

4. **Не используйте `void*` без необходимости — делайте типобезопасные структуры.**
   **Плохо:**

   ```c
   void process(void* data) { ... }
   ```

   **Хорошо:**

   ```c
   void process(int* data) { ... }
   ```

5. **Не смешивайте логику работы с памятью, I/O и бизнес-логику в одной функции.**
   **Плохо:**

   ```c
   int readAndSum(const char* filename) {
       FILE* f = fopen(filename, "r");
       int x, sum=0;
       while(fscanf(f,"%d",&x)==1) sum+=x;
       fclose(f);
       return sum;
   }
   ```

   **Хорошо:**

   ```c
   int sumArray(const int* arr, size_t n) {
       int sum = 0;
       for(size_t i=0; i<n; i++) sum += arr[i];
       return sum;
   }
   int* readFile(const char* filename, size_t* outSize) { ... }
   ```
