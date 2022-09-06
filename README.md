# pattern_prototype

## Цель патерна
### Проблема
Иногда надо скопировать объект, но надо это сделать "извне". Для этого нам надо создать новый объект класса и копоровать все поля в него.
но копирование "извне" не всегда возможно в реальности да и может это быть сложнее.
### Решение
Паттерн Прототип поручает создание копий самим копируемым объектам. Он вводит общий интерфейс для всех объектов, поддерживающих клонирование. Это позволяет копировать объекты, не привязываясь к их конкретным классам. Обычно такой интерфейс имеет всего один метод clone

## Плюсы и минусы
### Плюсы
+ Позволяет клонировать объекты, не привязываясь к их конкретным классам.
+ Меньше повторяющегося кода инициализации объектов.
+ Ускоряет создание объектов.
+ Альтернатива созданию подклассов для конструирования сложных объектов.
 ### Минусы
+ Сложно клонировать составные объекты, имеющие ссылки на другие 

![photo_2022-09-06_10-36-47](https://user-images.githubusercontent.com/108687865/188575356-697086d9-d744-4287-a377-cf6ee0e8d57c.jpg)

## Пример
```cpp
#include <iostream>
#include <string>
using namespace std;


class Object3D abstract
{
protected:
  string name;
  Object3D(string _name) : name{ _name } {}
public:
  virtual void setName(string _name) {
    name = _name;
  }
  virtual string getName() const {
    return name;
  }
  virtual Object3D* clone() const = 0;
  virtual void draw() const = 0;
};
class Box : public Object3D
{
public:
  Box(string _name) :Object3D{ _name } {}

  Object3D* clone() const override {
    cout << "box clone" << endl;
    return new Box(name);
  }
  void draw() const override {
    cout << "draw box with name: " << name << endl;
  };
};
class Sphere : public Object3D
{
public:
  Sphere(string _name) :Object3D{ _name } {}

  Object3D* clone() const override {
    cout << "sphere clone" << endl;
    return new Box(name);
  }
  void draw() const override {
    cout << "draw sphere with name: " << name << endl;
  };
};
int main()
{
  // prototype
  Object3D* box1 = new Box("Big box");

  // clone
  Object3D* box2 = box1->clone();
  box2->setName("Big box clone");

  box1->draw();
  box2->draw();

  // prototype
  Object3D* sphere1 = new Sphere("Big sphere");

  // clone
  Object3D* sphere2 = sphere1->clone();
  sphere2->setName("Big sphere clone");

  sphere1->draw();
  sphere2->draw();
}
```
### Выводит в консоль
```
box clone
draw box with name: Big box
draw box with name: Big box clone
sphere clone
draw sphere with name: Big sphere
draw box with name: Big sphere clone
```

![ClassDiagram](https://user-images.githubusercontent.com/108687865/188584146-dd8ec2a3-d48b-4869-9e9e-e59acf1d7f31.jpg)

## Список типов
+ Базовый тип Object3D
+ Наследник Box
+ Наследник Sphere

## Аналогия
Приходит человек в бар и видит другого с какимто напитком.
Просит у бармена такого же (копию или клон) напитка.
Бармен денлает такой же и врочает тому кто его просил.

![eab5a8767b24711d67871bf5e0c14209](https://user-images.githubusercontent.com/108687865/188563073-893a72e7-edad-4ebf-958c-ad4d2b9ae6d4.jpg)
