# Reto7_POO
Se actualiza el menu del restaurante, creando las clases OrderIterable y OrderIterator, que sirven para recorrer la lista de elementos del menú que fueron seleccionados por el usuario.
Al correr el archivo se pide al usuario elegir su menú, método de pago y finalmente, usando el iterador, se muestran todos los elementos, siendo ordeneados por el precio en orden ascendente.


``` python
class Order_Iterable():
    def __init__(self, order: Order) -> None:
        self.order = order
        
    # When implementing an iterable, implementing the __iter__ method creates an iterator object in every call.
    def __iter__(self):
        return Order_Iterator(self.order)
    
class Order_Iterator():
    def __init__(self, order: Order) -> None:
        self.items_list = order.items_list
        self.index = 0
    
    def __next__(self):
        if self.index == len(self.items_list):
            raise StopIteration()
        item = self.items_list[self.index]
        self.index += 1
        return item
        
    # When implementing an iterator, it is a good practice to implement the __iter__ method as well, 
    # so the iterator can be used in for loops for example.
    def __iter__(self): 
        return self
```

Para el orden se definió el método ``` __lt__ ``` en la clase MenuItem para así poder comparar dos items por su precio, siendo estos objetos de esta clase.

``` python
class MenuItem:
    def __init__(self, name, price, method):
        self.name = name
        self.price = price
        self.method = method

    def __lt__(self, other): 
        return self.price < other.price
```

Acá se define el método sort_items_list de la clase Order:
``` python
class Order():
    def __init__(self, items_list, payment) -> None:
        self.items_list = items_list
        self.payment = payment

    def calculate_total(self):  
        total = 0
        for item in self.items_list:
            try:                                    #Some items don't have size atribute
                if item.size == "2":
                    total += item.price * 1.25
                else:
                    total += item.price
            except:
                total += item.price
        return total

    def apply_discount(self, total):
        personal_combo = set(["CheeseBurger", "Fries", "Soda"])
        personal_combo2 = set(["Pizza", "Milkshake"])
        familiar_combo = set(["ChickenNuggets", "HotDog", "Steak", "Salad"])
        items = []
        for item in self.items_list:
            items.append(item.name)
        items = set(items)
        
        #Combos discounts
        if items == personal_combo:
            total = total * 0.9
            print("Personal combo discount applied")
        elif items == personal_combo2:
            total = total * 0.8
            print("Personal combo 2 discount applied")
        elif items == familiar_combo:
            total = total * 0.7
            print("Familiar combo discount applied")
        else:
            print("No combo discount applied")
        
        #Payments
        if self.payment == "cash":
            total = total * 0.95
            print("Cash payment applied")
        elif self.payment == "card":
            total = total * 1.05    
            print("Card payment applied")
        else:
            print("Nequi payment applied")
        return total
    
    def sort_items_list(self):
        return Order(sorted(self.items_list), self.payment)
```

