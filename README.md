import turtle
import math
import random
import time

# Ventana
screen = turtle.Screen()
screen.bgcolor("black")
screen.title("Sistema Solar")
screen.setup(900, 900)
screen.tracer(0)

t = turtle.Turtle()
t.hideturtle()
t.penup()

stars = turtle.Turtle()
stars.hideturtle()
stars.penup()

texto = turtle.Turtle()
texto.hideturtle()
texto.penup()
texto.color("white")

# estrellas
estrellas = [(random.randint(-450,450),
              random.randint(-450,450),
              random.randint(1,3)) for _ in range(120)]

# FRASES
frase_sol = "Gracias por todos los momentos y enseñanzas, son una gran inspiración"

planetas = [
    ("Mercurio", 4, 60, "gray", 4, "Son las mejores"),
    ("Venus", 6, 90, "orange", 3, "Gracias por todo"),
    ("Tierra", 6, 120, "blue", 2.5, "Las voy a extrañar mucho"),
    ("Marte", 5, 150, "red", 2, "Las aprecio mucho"),
    ("Jupiter", 12, 200, "brown", 1, "Mucha suerte y éxito en todo"),
    ("Saturno", 10, 250, "gold", 0.8, "Fue un honor conocerlas"),
    ("Urano", 8, 290, "lightblue", 0.6, "Pórtense bien"),
    ("Neptuno", 8, 330, "blue", 0.5, "Siempre las recordaré"),
]

angulo = 0
ultimo_click = 0
posiciones = []

def dibujar():
    global angulo, posiciones
    t.clear()
    stars.clear()
    posiciones = []

    # estrellas
    for x,y,s in estrellas:
        stars.goto(x,y)
        stars.dot(s,"white")

    # SOL
    t.goto(0,0)
    t.dot(40,"yellow")

    # guardar posición del sol
    posiciones.append((0, 0, 20, frase_sol))

    # planetas
    for nombre, radio, dist, color, vel, frase in planetas:

        t.goto(0,-dist)
        t.pendown()
        t.pencolor("#222")
        t.circle(dist)
        t.penup()

        a = math.radians(angulo * vel)
        x = dist * math.cos(a)
        y = dist * math.sin(a)

        t.goto(x,y)
        t.dot(radio*2, color)

        posiciones.append((x, y, radio*2, frase))

    screen.update()

def mostrar_texto(frase):
    texto.clear()
    texto.goto(0,350)
    texto.write(frase, align="center", font=("Arial", 14, "bold"))

def click(x, y):
    global ultimo_click
    ahora = time.time()

    if ahora - ultimo_click < 0.3:
        for px, py, r, frase in posiciones:
            if math.hypot(x-px, y-py) < r:
                mostrar_texto(frase)

    ultimo_click = ahora

def animar():
    global angulo
    angulo += 1
    dibujar()
    screen.ontimer(animar, 40)

screen.onclick(click)

animar()
turtle.done()
