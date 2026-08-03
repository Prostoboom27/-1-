#include <SFML/Graphics.hpp>
#include <SFML/Audio.hpp>
#include<iostream>
#include <time.h>
using namespace sf;

const int height = 18; // высота игрового поля
const int width = 9; // ширина игрового поля

int Playing_area[height][width] = { 0 }; // игровое поле

// Массив фигурок-тетрамино
int Shapes[7][4] =
{
	1,3,5,7, // I
	2,4,5,7, // Z
	3,5,4,6, // S
	3,5,4,7, // T
	2,3,5,7, // L
	3,5,7,6, // J
	2,3,4,5, // O
};

struct Coordinates
{
	int x, y;
} a[4], b[4];

//  Проверка на выход из игрового поля  //
    bool verify()
    {
	for (int i = 0; i < 4; i++)
	{

		if (a[i].x < 0 || a[i].x >= width || a[i].y >= height)
		{
			return 0;
		}
		else if (Playing_area[a[i].y][a[i].x])
		{
			return 0;
		}
	}
	return 1;

    }

	// Проверка верхней линии (Конец игры) //
	bool verify_finish_game()
	{
		for (int i = 0; i < height; ++i)
		{
			if (Playing_area[1][i] != 0)
			{
				return true;
			}
		}
		return false;
	}
	
int main()
{
	char logo[] =
		"************************************************\n"
		"* Nizhny Novgorog State Technical University   *\n"
		"*          Kursovay rabota 'Tetris'            *\n"
		"* Performed student 21-IVT-3 Sychev E. S.      *\n"
		"************************************************\n";
	std::cout << logo;
	
	char GaOv[] =
		"*****************************************\n"
		"*                                       *\n"
		"*           Sorry, you lost             *\n"
		"*     Learn to play, then come back     *\n"
		"*     This game is not for everyone     *\n"
		"*           You're a loser !!!          *\n"
		"*                                       *\n"
		"*****************************************\n";

	srand(time(0));
	// Фоновая музыка //
	Music Music_fon;
	Music_fon.openFromFile("C://Users//syche//Desktop//Tetris//Debug//Tetris.wav");
	Music_fon.play();


	RenderWindow window(VideoMode(326, 600), "TETRIS");

	//  Создание и загрузка текстуры  //
	Texture piece, background;
	piece.loadFromFile("C://Users//syche//Desktop//Tetris//Debug//Tint.jpg");
	background.loadFromFile("C://Users//syche//Desktop//Tetris//Debug//Background.jpg");

	//  Создание спрайта  //
	Sprite sprite(piece),sprite_background(background);


	//  Вырезаем из спрайта отдельный квадратик размером 18х18 пикселей  //
	sprite.setTextureRect(IntRect(0, 0, 30, 30));

	//  Переменные для горизонтального перемещения и вращения  //
	int HorizMove = 0;
	bool spin = 0;
	int colorNum = 1; 
	bool beginGame = true; 
	int n = rand() % 7; // какая фигура //

	//  Переменные для таймера и задержки  //
	float timer = 0, latency = 0.3;

	// Часы (таймер)  //
	Clock clock;


	//  Выполняется, пока открыто окно  //
	while (window.isOpen())
	{
		if (verify_finish_game())
		{
			system("cls");
			std::cout << GaOv;
			break;
			system("Pause");
		}

		//  Если дошли до верхней точки, программа закрывается  //
		if (verify_finish_game())
		{
			break;
		}
		
		

		// Получаем время, прошедшее с начала отсчета и конвертируем его в секунды  //
		float time = clock.getElapsedTime().asSeconds();
		clock.restart();
		timer += time;

		// Обрабатываем очередь событий в цикле  //
		Event event;
		while (window.pollEvent(event))
		{
			//  Если пользователь нажал "Крестик"  //
			if (event.type == Event::Closed) 
			{
				//  Закрываем его  //
				window.close();
			}

			//  Была нажата кнопка на клавиатуре?  //
			if (event.type == Event::KeyPressed) 
			{
				//  Если эта кнопка – стрелка вверх  //
				if (event.key.code == Keyboard::Up)
				{
					spin = true;
				}
				//  Если эта кнопка – стрелка влево  // 
				else if (event.key.code == Keyboard::Left)
				{
					HorizMove = -1;
				}
				//  Если эта кнопка – стрелка вправо  //
				else if (event.key.code == Keyboard::Right)
				{
					HorizMove = 1;
				}
			}
		}

		   //  Нажали кнопку "вниз"?  //
		//  Ускоряем падение тетрамино  //

		if (Keyboard::isKeyPressed(Keyboard::Down)) 
		{
			latency = 0.05;
		}

		// Горизонтальное перемещение тетрамино //

		for (int i = 0; i < 4; i++) 
		{ 
			b[i] = a[i]; 
			a[i].x += HorizMove;
		}

		//  Вышли за пределы поля после перемещения?  //
		      // Возвращаем старые координаты  //
		if (!verify())
		{
			for (int i = 0; i < 4; i++) 
			{
				a[i] = b[i];
			}
		}


		//  Вращение фигурки тетрамино //
		if (spin)
		{
			Coordinates p = a[1]; // задаем центр вращения
			for (int i = 0; i < 4; i++)
			{
				int x = a[i].y - p.y; //y-y0
				int y = a[i].x - p.x; //x-x0
				a[i].x = p.x - x;
				a[i].y = p.y + y;
			}

			// Вышли за пределы поля после поворота? //
			//  Возвращаем старые координаты //
			if (!verify())
			{
				for (int i = 0; i < 4; i++) 
				{
					a[i] = b[i];
				}
			}

		}

		//  Движение тетрамино вниз  //
		if (timer > latency)
		{
			for (int i = 0; i < 4; i++) 
			{ 
				b[i] = a[i]; 
				a[i].y += 1; 
			}
			if (!verify())
			{
				for (int i = 0; i < 4; i++) 
				{
					Playing_area[b[i].y][b[i].x] = colorNum;
				}
				
				colorNum = 1 + rand() % 7;
				n = rand() % 7;
				
				for (int i = 0; i < 4; i++)
				{
					a[i].x = Shapes[n][i] % 2;
					a[i].y = Shapes[n][i] / 2;
				}

			}

			timer = 0;
		}
		// Проверка нижней линии //
		int k = height - 1;
		for (int i = height - 1; i > 0; i--)
		{
			int count = 0;
			for (int j = 0; j < width; j++)
			{
				if (Playing_area[i][j])
				{
					count++;
				}
				Playing_area[k][j] = Playing_area[i][j];
			}
			if (count < width)
			{
				k--;
			}
		}

		// Первое появление тетрамино на поле //
		if (beginGame)
		{
			beginGame = false;
			n = rand() % 7;
			for (int i = 0; i < 4; i++)
			{
				a[i].x = Shapes[n][i] % 2;
				a[i].y = Shapes[n][i] / 2;
			}
		}
		HorizMove = 0;
		spin = 0;
		latency = 0.3;


		// Задаем цвет фона - белый
		window.clear(Color::White);
		window.draw(sprite_background);

		for (int i = 0; i < height; i++)
			for (int j = 0; j < width; j++)
			{
				if (Playing_area[i][j] == 0)
				{
					continue;
				}
				sprite.setTextureRect(IntRect(Playing_area[i][j] * 30, 0, 30, 30));
				sprite.setPosition(j * 30, i * 30);
				sprite.move(28, 31); // смещение
				window.draw(sprite);
			}

		for (int i = 0; i < 4; i++)
		{
			// Разукрашиваем тетрамино
			sprite.setTextureRect(IntRect(colorNum * 30, 0, 30, 30));

			// Устанавливаем позицию каждого кусочка тетрамино
			sprite.setPosition(a[i].x * 30, a[i].y * 30);

			sprite.move(28, 31); // смещение
			// Отрисовка спрайта
			window.draw(sprite);
		}
		// Отрисовка окна
		window.display();
	}
	return 0;
}
