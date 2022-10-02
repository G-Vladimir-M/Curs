# Curs
import pygame
import time
import random

pygame.init()           # подключаем библиотеку Pygame для работы с экраном и клавиатурой

white = (255, 255, 255)
yellow = (255, 255, 102)
black = (0, 0, 0)
red = (213, 50, 80)
green = (0, 255, 0)
blue = (50, 153, 213)
dis_width = 600             # задаём параметры поля
dis_height = 600            # задаём параметры поля
dis = pygame.display.set_mode((dis_width, dis_height))  # функцией (display.set_mode) создаём квадрат с уже заданными размерами
pygame.display.set_caption('Кобра')                    # даём название Змейки
clock = pygame.time.Clock()     # используем функцию объекта часов для отслеживания времени
snake_block = 10    # указываем величину сдвига змейки при нажатии на клавишу
snake_speed = 15    # ограничитель скорости движения змейки
font_style = pygame.font.SysFont("bahnschrift", 25) # указываем название и размер шрифта для сообщения о завершении игры
score_font = pygame.font.SysFont("comicsansms", 35) # указываем название и размер шрифта для сообщения о счёте

def Your_score(score):  # создаём функцию для счета
    value = score_font.render("Счёт: " + str(score), True, yellow)
    dis.blit(value, [0, 0])

def our_snake(snake_block, snake_list):
    for x in snake_list:
        pygame.draw.rect(dis, black, [x[0], x[1], snake_block, snake_block])

def message(msg, color):    # создаём функцию которая будет показывать сообщения на игровом поле
    mesg = font_style.render(msg, True, color)
    dis.blit(mesg, [dis_width / 6, dis_height / 3])


def gameLoop():  # Описываем всю игровую логику в одной функции.
    game_over = False
    game_close = False
    x1 = dis_width / 2  # указываем начальное значение положение змейки по осям (х у) исходя из размера экрана
    y1 = dis_height / 2
    x1_change = 0   # создали переменную, которой в цикле будет ирисваиваться положение змейки по оси - Х
    y1_change = 0   # создали переменную, которой в цикле будет ирисваиваться положение змейки по оси - Y
    snake_List = [] # создаём список который будет хранить длину змейки
    Length_of_snake = 1
    foodx = round(random.randrange(0, dis_width - snake_block) / 10.0) * 10.0   # Создаём переменную, которая будет указывать расположение еды по оси х.
    foody = round(random.randrange(0, dis_width - snake_block) / 10.0) * 10.0   # Создаём переменную, которая будет указывать расположение еды по оси y.
    while not game_over:    # функция (while) не даст закрыть игру сразу, а будет продолжаться бесконечно.
        while game_close == True:
            dis.fill(white)
            message("Вы проиграли! Q- выход, C- новая игра", red)
            Your_score(Length_of_snake - 1)
            pygame.display.update()
            for event in pygame.event.get():    # функцией (event.get()) возвращаем в терминал все события кот. происходят в игре
                if event.type == pygame.KEYDOWN:    # расписываем управление змейкой при помощи класса (pygame.KEYDOWN)
                    if event.key == pygame.K_q:     # считываем направление движение с клавиатуры.
                        game_over = True
                        game_close = False
                    if event.key == pygame.K_c:
                        gameLoop()
        for event in pygame.event.get():    # создаём ручное закрытие игры
            if event.type == pygame.QUIT:
                game_over = True
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    x1_change = -snake_block
                    y1_change = 0
                elif event.key == pygame.K_RIGHT:
                    x1_change = snake_block
                    y1_change = 0
                elif event.key == pygame.K_UP:
                    y1_change = -snake_block
                    x1_change = 0
                elif event.key == pygame.K_DOWN:
                    y1_change = snake_block
                    x1_change = 0
        if x1 >= dis_width or x1 < 0 or y1 >= dis_height or y1 < 0:
            game_close = True   # если координаты змейки выходят за рамки поля, то игра заканчивается.
        x1 += x1_change     # записываем новое положение змейки по осям (х у)
        y1 += y1_change
        dis.fill(white)
        pygame.draw.rect(dis, green, [foodx, foody, snake_block, snake_block])
        snake_Head = []     # список в кот.храниться длина змейки в движении
        snake_Head.append(x1)   # добавляем значение в список при изменении по осям (х у)
        snake_Head.append(y1)
        snake_List.append(snake_Head)
        if len(snake_List) > Length_of_snake:   # удаляем первый элемент в списке длины змейки чтобы она не увеличивалась сама по себе
            del snake_List[0]
        for x in snake_List[:-1]:
            if x == snake_Head:
                game_close = True
        our_snake(snake_block, snake_List)
        Your_score(Length_of_snake - 1)
        pygame.display.update()
        if x1 == foodx and y1 == foody:     # говорим, что если координаты еды совпадут с координатами головы, то еда снова появляется
            foodx = round(random.randrange(0, dis_width - snake_block) / 10.0) * 10.0
            foody = round(random.randrange(0, dis_height - snake_block) / 10.0) * 10.0
            Length_of_snake += 1        # а длина змейки при этом увеличится на 1 показатель
        clock.tick(snake_speed)
    pygame.quit()       # отключаем игровое поле
    quit()

gameLoop()
