/* Подключение библиотек */
#include <cstdlib>
#include <iostream>
#include <conio.h>
#include <string.h>
#include <ctype.h>

/* Прототипы функций */
int calculate_age(int bd, int bm, int by, int cd, int cm, int cy);
int is_leap_year(int year);
int is_valid_date(int day, int month, int year);

/* Главная функция */
int main(int argc, char *argv[])
{
 
/* Настройка консоли для русского языка */
    system("@echo off");
    system("chcp 1251 >nul");
    
    char birth_date[11], current_date[11];
    int birth_day, birth_month, birth_year;
    int current_day, current_month, current_year;
    int age;
    char ending[10];
    
    printf("ПРОГРАММА ДЛЯ ВЫЧИСЛЕНИЯ ВОЗРАСТА\n\n");
    
/* Ввод даты рождения */
    printf("Введите дату рождения (дд.мм.гггг): ");
    scanf("%10s", birth_date);
    
/* Ввод текущей даты */
    printf("Введите текущую дату (дд.мм.гггг): ");
    scanf("%10s", current_date);
    
/* Проверка формата ввода */
    if(strlen(birth_date) != 10 || strlen(current_date) != 10)
    {
        printf("Ошибка! Формат: дд.мм.гггг (10 символов)\n");
        printf("\nДля завершения программы нажмите любую клавишу...\n");
        getch();
        return EXIT_FAILURE;
    }
    
if(birth_date[2] != '.' || birth_date[5] != '.' || 
 current_date[2] != '.' || current_date[5] != '.')
    {
        printf("Ошибка! Используйте точки как разделители\n");
        printf("\nДля завершения программы нажмите любую клавишу...\n");
        getch();
        return EXIT_FAILURE;
    }
    
/* Преобразование строк в числа */
    char day_str[3], month_str[3], year_str[5];
    
/* Для даты рождения */
    day_str[0] = birth_date[0]; day_str[1] = birth_date[1]; day_str[2] = '\0';
    month_str[0] = birth_date[3]; month_str[1] = birth_date[4]; month_str[2] = '\0';
    year_str[0] = birth_date[6]; year_str[1] = birth_date[7]; 
    year_str[2] = birth_date[8]; year_str[3] = birth_date[9]; year_str[4] = '\0';
    
    birth_day = atoi(day_str);
    birth_month = atoi(month_str);
    birth_year = atoi(year_str);
    
/* Для текущей даты */
    day_str[0] = current_date[0]; day_str[1] = current_date[1]; day_str[2] = '\0';
    month_str[0] = current_date[3]; month_str[1] = current_date[4]; month_str[2] = '\0';
    year_str[0] = current_date[6]; year_str[1] = current_date[7];
    year_str[2] = current_date[8]; year_str[3] = current_date[9]; year_str[4] = '\0';
    
    current_day = atoi(day_str);
    current_month = atoi(month_str);
    current_year = atoi(year_str);
    
/* Проверка корректности дат */
    if(!is_valid_date(birth_day, birth_month, birth_year))
    {
        printf("Ошибка! Некорректная дата рождения\n");
        printf("\nДля завершения программы нажмите любую клавишу...\n");
        getch();
        return EXIT_FAILURE;
    }
    
    



if(!is_valid_date(current_day, current_month, current_year))
    {
        printf("Ошибка! Некорректная текущая дата\n");
        printf("\nДля завершения программы нажмите любую клавишу...\n");
        getch();
        return EXIT_FAILURE;
    }
    
/* Проверка порядка дат */
    if(birth_year > current_year || 
       (birth_year == current_year && birth_month > current_month) || 
       (birth_year == current_year && birth_month == current_month && 
        birth_day > current_day))
    {
        printf("Ошибка! Дата рождения позже текущей даты\n");
        printf("\nДля завершения программы нажмите любую клавишу...\n");
        getch();
        return EXIT_FAILURE;
    }
    
/* Вычисление возраста с помощью пользовательской функции */
    age = calculate_age(birth_day, birth_month, birth_year,
                        current_day, current_month, current_year);
    
/* Выбор правильной формы слова */
    if(age % 100 >= 11 && age % 100 <= 14)
    {
        strcpy(ending, "лет");
    }
    else if(age % 10 == 1)
    {
        strcpy(ending, "год");
    }
    else if(age % 10 >= 2 && age % 10 <= 4)
    {
        strcpy(ending, "года");
    }
    else
    {
        strcpy(ending, "лет");
    }

/* Вывод результата */
  


    printf("\n========================================\n");
    printf("               РЕЗУЛЬТАТ               \n");
    printf("========================================\n");
    printf("          Возраст: %d %s\n", age, ending);
    printf("========================================\n");
    
/* Завершение программы*/
    printf("\nДля завершения программы нажмите любую клавишу...\n");
    getch();
    return EXIT_SUCCESS;
}

/* ПОЛЬЗОВАТЕЛЬСКАЯ ФУНКЦИЯ Вычисление возраста в полных годах */
int calculate_age(int bd, int bm, int by, int cd, int cm, int cy)
{
    int age = cy - by;
    
/* Если день рождения еще не наступил в текущем году */
    if(cm < bm || (cm == bm && cd < bd))
    {
        age--;
    }
    
    return age;
}

/* Функция проверки високосного года */
int is_leap_year(int year)
{
    return (year % 400 == 0) || (year % 4 == 0 && year % 100 != 0);
}

/* Функция проверки корректности даты */
int is_valid_date(int day, int month, int year)
{

/* Проверка диапазонов */
    if(year < 1900 || year > 2100) return 0;
    if(month < 1 || month > 12) return 0;
    
/* Количество дней в месяцах */
    int days_in_month[] = {31, 28, 31, 30, 31, 30, 
                           31, 31, 30, 31, 30, 31};
    
    

/* Корректировка для февраля в високосный год */
    if(month == 2 && is_leap_year(year))
    {
        days_in_month[1] = 29;
    }
    
    /* Проверка дня */
    if(day < 1 || day > days_in_month[month - 1])
    {
        return 0;
    }
    
    return 1;
}    
