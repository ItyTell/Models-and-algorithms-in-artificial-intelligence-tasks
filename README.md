# Models-and-algorithms-in-artificial-intelligence-tasks
Labs for the course "Models and algorithms in artificial intelligence tasks". Mostly done in Jupyter notebook




Ми отримали файл такого вигляду:

     0 8.717e+004
     0.000488 6.153e+004
     0.000976 1.758e+004
     0.001464-1.49e+004
     0.001952    7114
     0.00244 1.09e+005
     0.002928 2.697e+005
     0.003416 4.207e+005
     0.003904 4.679e+005
     0.004392 3.447e+005
     0.00488 6.281e+004
     0.005368-2.765e+005
     0.005856-5.377e+005

.................................................

Для нормальної обробки файла його було відформатовано за допомогою текстових макросів (додали пробіли перед від'ємними числами)

     0 8.717e+004
     0.000488 6.153e+004
     0.000976 1.758e+004
     0.001464 -1.49e+004
     0.001952    7114
     0.00244 1.09e+005
     0.002928 2.697e+005
     0.003416 4.207e+005
     0.003904 4.679e+005
     0.004392 3.447e+005
     0.00488 6.281e+004
     0.005368 -2.765e+005
     0.005856 -5.377e+005

    ................................................

Проаналізуємо цей файл у Jupyter notebook

Одразу напишу всі потрібні нам імпорти:

import numpy as np
from matplotlib import pyplot as plt

Загрузимо файл, записавши перші числа у список t (від слова час) і другі числа у список x ( від слова з трьох букв):

    file = open("Fragment Full Data D1.prn", "r")
    x = []
    t = []
    for l in file:
        l = l.split()
        t.append(float(l[0]))
        x.append(float(l[1]))
    file.close()
    x = np.array(x)

Подивимось на те, що собою являють перші числа ( проаналізувавши їх графік):

    fig, ax = plt.subplots()
    plt.title("Перші числа", color = 'Orange', fontsize=15)
    ax.set_facecolor('#232323')
    ax.plot(t, color = 'red')
    ax.tick_params(labelcolor='tab:orange')
    plt.show()
    
![58eb3eb8-3f51-469c-8613-89189bac0d65](https://user-images.githubusercontent.com/89577338/234517738-adc421e5-73b4-4e43-8cf6-07396fd3f001.png)

Очевидно, що це часова пряма (звідки є t), насправді Мостовий те саме сказав, хз навіщо я оце клоунячив. 

Він також сказав, що х це сигнал прийшовший у момент t.

Тепер вже побудуємо графік x(t):

    fig, ax = plt.subplots()
    plt.title("Данний сигнал", color = 'Orange', fontsize=15)
    ax.set_facecolor('#232323')
    ax.plot(t,x, color = 'red')
    ax.tick_params(labelcolor='tab:orange')
    plt.show()
    
![98e8c44b-eb35-4ab4-8fc8-d7f8b8fb4912](https://user-images.githubusercontent.com/89577338/234518016-f44782c9-27cc-4786-bae6-8abaae609de2.png)

Була поставлена задача зостосувати до цих данних Фурьє перетворення.

Коротко на прикладі розберемо, що це за шняга така.

Нехай у нас э такий сигнал (позначення ті самі):

    dt = 0.001
    t = np.arange(0, 0.2, dt)
    x = np.sin(2 * np.pi * 30 * t) + np.sin(2 * np.pi * 70 * t)

Я вибрав частоти 2 * np.pi * 30 та 2 * np.pi * 70.

Застосуємо перетворення Фур'є:

    n = len(t)                                                                 # кількість вимірів (кількість записів у файлі)
    fhat =np.fft.fft(x, n)                                              # функція з бібліотеки шо робить Фур'є
    PSD = np.real(fhat * np.conj(fhat) )                        # модуль результата перетворення 
                                                                                   # (щоб позбутися комплексних значень)
    freq = (1 / dt * n) * np.arange(n)                             # масив частот
    L = np.arange(1, np.floor(n/2),  dtype = 'int')          # беремо лише ліву половину графіка
                                                                                   # бо він семетричний

Отримуємо графік:

![516c7f05-9d64-4549-89e2-7b16fd8d3dbf](https://user-images.githubusercontent.com/89577338/234518197-a3ad9f95-3c40-4c7b-ae2c-129f6b7a8159.png)

Це саме ті дві частоти. Щоб краще зрозуміти, яка тут махінація вийшла пограємось з параметрами, що я взяв з голови на початку (там де були два сінуси)

Додамо наприклад амплітуду 2 до другого сінуса 

    dt = 0.001
    t = np.arange(0, 0.2, dt)
    x = np.sin(2 * np.pi * 30 * t) + 2 * np.sin(2 * np.pi * 70 * t)

Отримаємо:


![зображення](https://user-images.githubusercontent.com/89577338/234519311-96aa2d0b-b40c-479c-93be-7852a844b793.png)

Можна так само додати амплітуду і до першого сінуса, вийде аналогічно:


![зображення](https://user-images.githubusercontent.com/89577338/234519336-a27ec61f-dbdd-4aa4-a75f-489b29dca881.png)

Поняли, да

Також можна наприклад поміняти період (частоту) синуса:

    dt = 0.001
    t = np.arange(0, 0.2, dt)
    x = np.sin(2 * np.pi * 30 * t) + 2 * np.sin(2 * np.pi * 300 * t)

Отримаємо:


![зображення](https://user-images.githubusercontent.com/89577338/234519385-0000a701-8f2c-4662-930e-d556ab52c0cb.png)

Поняли да? Воно рухається !!!

Ну і короче так можна визначити приблизні частоти, що утворюють наш сигнал, ну і звісно там буде шум адже наш сигнал із реального світу тому він буде кривий трохи.

Повернемось до нашого сигналу. У нас був список t з часом та список x з самим сигналом. Зробимо претворення:

    n = len(t)
    fhat =np.fft.fft(x, n)
    PSD = np.real(fhat * np.conj(fhat) )/ n
    freq = (1/t[n - 1]) * np.arange(n)            # там були моменти часу dt * n а тут просто t[n-1]
    L = np.arange(1, np.floor(n/2),  dtype = 'int')

При побудові графіка отримаємо:

    fig, ax = plt.subplots()
    plt.title("Дані отримані після перетворення Фурьє", color = 'Orange')
    ax.set_facecolor('#232323')
    ax.plot(freq[L], PSD[L], color = 'red')
    ax.tick_params(labelcolor='tab:orange')
    plt.show()
    
![зображення](https://user-images.githubusercontent.com/89577338/234519418-2d819ebd-ff17-4e12-a9ff-03d8ff22a429.png)

Зовсім не кравиво

Якщо взяти наприклад частоти до 150 то буде видно краще:

    L = np.arange(1, np.floor(3*n/ 40),  dtype = 'int')  
    # було n/2 і це було до 1000, стало 3n/40  тобто до 150
    fig, ax = plt.subplots()
    plt.title("Дані отримані після перетворення Фурьє", color = 'Orange')
    ax.set_facecolor('#232323')
    ax.plot(freq * L, PSD[L], color = 'red')
    ax.tick_params(labelcolor='tab:orange')
    plt.show()

Отримуємо:


![зображення](https://user-images.githubusercontent.com/89577338/234519446-c07ca2ce-4cff-4a2e-b98e-2c1641689903.png)
