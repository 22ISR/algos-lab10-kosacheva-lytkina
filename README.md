# Лабораторная работа №10

## Задание

Разработать графический интерфейс для списка задач (TODO-листа) с возможностью добавления, удаления и выбора задач.

1. Создать окно приложения с заголовком "Мой TODO-лист" и фиксированным размером.

2. Добавить Listbox для отображения списка задач.

3. Добавить поле ввода (Entry) для ввода новой задачи.

4. Добавить кнопки:

    "Добавить" — добавляет задачу в список.
    "Удалить" — удаляет выбранную задачу.
    "Очистить всё" — полностью очищает список.

<img src="./.repo/todo.png" />

## Теория

[Документация](https://metanit.com/python/tkinter/2.12.php)

**Listbox** — это виджет в tkinter, который отображает список элементов. Пользователь может добавлять, выбирать и удалять элементы.

### Создание Listbox

```python
listbox = tk.Listbox(root, width=30, height=5)
listbox.pack(pady=10)
```

### Добавление элементов

```python
listbox.insert(tk.END, "Элемент 1")
listbox.insert(tk.END, "Элемент 2")
listbox.insert(tk.END, "Элемент 3")

root.mainloop()
```

- _listbox = tk.Listbox(root, width=30, height=5)_ — создаёт список.
- _listbox.insert(tk.END, "Элемент")_ — добавляет элемент в конец списка.

### Выбор элемента в Listbox

Можно получить выделенный элемент с помощью curselection().

```python
def get_selected():
    selected = listbox.curselection()
    if selected:
        print("Выбрано:", listbox.get(selected[0]))

select_button = tk.Button(root, text="Выбрать", command=get_selected)
select_button.pack()
```

- _curselection()_ возвращает список индексов выделенных элементов.
- _get(index)_ возвращает текст элемента по индексу.

## Удаление элементов из Listbox

Удалить можно по индексу или все элементы сразу.

### Удаление выбранного элемента:

```python
def delete_selected():
    selected = listbox.curselection()
    if selected:
        listbox.delete(selected[0])

delete_button = tk.Button(root, text="Удалить", command=delete_selected)
delete_button.pack()
```

_Если ничего не выбрано, код ничего не делает._

### Очистка всего списка:

```python
listbox.delete(0, tk.END)
```



    import tkinter as tk
    from tkinter import ttk  

        class TodoApp:
        def __init__(self, root):
            self.root = root
            self.root.title("Список дел")
            self.root.geometry("400x400")  
            self.root.resizable(False, False)

            self.tasks = [] 

       
            self.task_entry = ttk.Entry(root, width=30) 
            self.task_entry.pack(pady=10)

            self.add_button = ttk.Button(root, text="Добавить", command=self.add_task)
            self.add_button.pack()

            self.task_listbox = tk.Listbox(root, width=40, height=10)
            self.task_listbox.pack(pady=10)

            self.delete_button = ttk.Button(root, text="Удалить", command=self.delete_task)
            self.delete_button.pack()

            self.clear_button = ttk.Button(root, text="Очистить всё", command=self.clear_all)
            self.clear_button.pack()

        
            style = ttk.Style()
            style.configure("TButton", padding=5, font=('Arial', 10)) 
            style.configure("TEntry", padding=5, font=('Arial', 10)) 

        def add_task(self):
            """Добавляет задачу из поля ввода в список и Listbox."""
            task = self.task_entry.get()
            if task:  
                self.tasks.append(task)
                self.task_listbox.insert(tk.END, task)  
                self.task_entry.delete(0, tk.END) 
            else:
                print("Пожалуйста, введите задачу.") 

         def delete_task(self):
            """Удаляет выбранную задачу из списка и Listbox."""
            try:
                selected_index = self.task_listbox.curselection()[0]  
                self.task_listbox.delete(selected_index)  
                del self.tasks[selected_index]  
                except IndexError:
                    print("Пожалуйста, выберите задачу для удаления.") 

        def clear_all(self):
            """Очищает весь список задач и Listbox."""
            self.tasks = []
            self.task_listbox.delete(0, tk.END)  


    if __name__ == "__main__":
    root = tk.Tk()
    app = TodoApp(root)
    root.mainloop()
