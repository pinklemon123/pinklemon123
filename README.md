- 👋 Hi, I’m @pinklemon123
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
pinklemon123/pinklemon123 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading;

namespace SnakeGame
{
    class Program
    {
        static void Main(string[] args)
        {
            // 游戏设置
            int width = 20;
            int height = 20;
            int score = 0;
            bool gameOver = false;

            // 蛇的初始位置
            List<Tuple<int, int>> snake = new List<Tuple<int, int>>() { Tuple.Create(width / 2, height / 2) };
            // 蛇的方向
            (int dx, int dy) direction = (0, -1); // 初始方向向上

            // 生成食物
            Random random = new Random();
            (int foodX, int foodY) = (random.Next(width), random.Next(height));

            // 游戏循环
            while (!gameOver)
            {
                // 清除控制台
                Console.Clear();

                // 绘制游戏界面
                for (int y = 0; y < height; y++)
                {
                    for (int x = 0; x < width; x++)
                    {
                        if (x == 0 || x == width - 1 || y == 0 || y == height - 1)
                        {
                            Console.Write("#"); // 边界
                        }
                        else if (snake.Contains(Tuple.Create(x, y)))
                        {
                            Console.Write("O"); // 蛇
                        }
                        else if (x == foodX && y == foodY)
                        {
                            Console.Write("F"); // 食物
                        }
                        else
                        {
                            Console.Write(" "); // 空地
                        }
                    }
                    Console.WriteLine();
                }

                // 显示得分
                Console.WriteLine($"Score: {score}");

                // 移动蛇
                (int newHeadX, int newHeadY) = (snake.First().Item1 + dx, snake.First().Item2 + dy);

                // 检查碰撞
                if (newHeadX < 0 ||X >= width || newHeadY < 0 || newHeadY >= height ||
                    snake.Skip(1).Contains(Tuple.Create(newHeadX, newHeadY))) // 碰到边界或自身
                {
                    gameOver = true;
                }
                else
                {
                    // 添加新的头部
                    snake.Insert(0, Tuple.Create(newHeadX, newHeadY));

                    // 检查是否吃到食物
                    if (newHeadX == foodX && newHeadY == foodY)
                    {
                        score++;
                        // 生成新的食物
                        (foodX, foodY) = (random.Next(width), random.Next(height));
                    }
                    else
                    {
                        // 移除尾部
                        snake.RemoveAt(snake.Count - 1);
                    }
                }

                // 等待用户输入以改变方向
                if (Console.KeyAvailable)
                {
                    var key = Console.ReadKey(true).Key;
                    switch (key)
                    {
                        case ConsoleKey.W:
                            if (dy != 1) direction = (0, -1); // 向上
                            break;
                        case ConsoleKey.S:
                            if (dy != -1) direction = (0, 1); // 向下
                            break;
                        case ConsoleKey.A:
                            if (dx != 1) direction = (-1, 0); // 向左
                            break;
                        case ConsoleKey.D:
                            if (dx != -1) direction = (1, 0); // 向右
                            break;
                    }
                }

                // 更新方向
                (dx, dy) = direction;

                // 延迟
                Thread.Sleep(100);
            }

            // 游戏结束
            Console.WriteLine("Game Over!");
            Console.WriteLine($"Final Score: {score}");
            Console.ReadKey();
        }
    }
}
