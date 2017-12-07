# Лекция 6

Темы, расмотренные в лекции:
- Интрефейсы взаимодействия пользователя и программы.
- Графические библиотеки.
- Библиотека GTK+.
- Примеры использования GTK+ 3.0.

## Интрефейсы взаимодействия пользователя и программы

Традиционно наиболее распространены два интерфейса взаимодействия пользователя с программой: интерфейс командной строки (CLI - command line intreface) и графический интерфейс пользователя (GUI - graphical user interface). Это не единственные способы взаимодействия (см. био- и нейроинтерфейсы), но они наиболее доступны на данный момент.

Как правило, CLI отпугивает простых людей, несмотря на то, что обладает рядом преимуществ перед GUI:
- Просто создается и редактируется.
- Программа крайне дополняется новым функционалом.
- Практически отсутствует необходимость в дизайне UX.
- Программу с CLI легко унифицировать с другими, т.е. легко привести к тому же способу взаимодействия пользователя с программой, что и в других ваших (или даже не ваших) проектах.

Но есть и ряд очевидных недостатков:
- Программы с CLI **очень** пугают обычных пользователей.
- Как правило, CLI тяжело пользоваться, если ты не значешь функционал программы досконально, так как нет вспомогательных "якорей" в виде иконок, которые обозначают доступные действия.
- GUI открывает широкие возможности для UX-дизайна и, как следствие, вы можете ускорить или сделать более удобной работу пользователя с программой.
- Внешний вид приложения часто работает продающим фактором (особенно в проектах с широкой аудиторией).

## Графические библиотеки

### OpenGL

[OpenGL](https://www.opengl.org/) - графическая библиотека, предназначенная для непосредственного взаимодействия с GPU и обладающая API на C. Как правило, для создания GUI используется какая-либо надстройка над ней, позволяющая работать с виджетами, поскольку сама OpenGL предназначена для работы с графикой на гораздо более низком уровне.

### freeglut

[freeglut](http://freeglut.sourceforge.net/) - кроссплатформенный менеджер окон и мыши/клавиатуры. Его API является надмножеством над GLUT (OpenGL Utility Toolkit, более не поддерживается), но при этом freeglut более современный и стабильный.

Не предназначен для управления виджетами, а скорее для создания нескольких окон для взаимодействия с OpenGL и работы с клавиатурой/мышью, например, в играх.

### DISLIN

[DISLIN](http://www.mps.mpg.de/dislin/) - библиотека для визуализации научных вычислений, доступна к использованию из C.

### Cairo

[Cairo](https://www.cairographics.org/) - многоцеливая библиотека для создания графических элементов. Есть API на C.

### Allegro 5

[Allegro 5](http://liballeg.org/) - кроссплатформенная библиотека с API на C, предназначеная для работы с мультимедиа, главным образом с играми.

### Simple DirectMedia Layer

[Simple DirectMedia Layer](http://www.libsdl.org/) (SDL) - кроссплатформенная библиотека с API на C, предназначенная для работы с мультимедиа. Наиболее часто все же используется для создания игр.

## GTK

[GTK+](https://www.gtk.org/) (GIMP Toolkit) - библиотека непосредственно предназначенная для создания GUI. Позволяет создавать и управлять различными виджетами. Несмотря на то, что практически все библиотеки для управления виджетами, в том числе и GTK, ориентированы на работы в ООП, доступен API на C.

### Установка GTK+ 3-ей версии.

Исходники GTK+ 3.22 доступны по [ссылке](http://ftp.gnome.org/pub/gnome/sources/gtk+/3.22/). Для установки необходими скачать tar-архив, распаковать его и запустить `./configure --help` для получения списка опций и дальнейших инструкций.

### Компиляция программ с использованием GTK_

Компиляция в gcc производится при помощи команды:

```
gcc `pkg-config --cflags gtk+-3.0` -o example-0 example-0.c `pkg-config --libs gtk+-3.0`
```

## Примеры использования GTK+ 3.0

### Hello, World!

```C
#include <gtk/gtk.h>

static void
print_hello (GtkWidget *widget,
             gpointer   data)
{
  g_print ("Hello World\n");
}

static void
activate (GtkApplication *app,
          gpointer        user_data)
{
  GtkWidget *window;
  GtkWidget *button;
  GtkWidget *button_box;

  window = gtk_application_window_new (app);
  gtk_window_set_title (GTK_WINDOW (window), "Window");
  gtk_window_set_default_size (GTK_WINDOW (window), 200, 200);

  button_box = gtk_button_box_new (GTK_ORIENTATION_HORIZONTAL);
  gtk_container_add (GTK_CONTAINER (window), button_box);

  button = gtk_button_new_with_label ("Hello World");
  g_signal_connect (button, "clicked", G_CALLBACK (print_hello), NULL);
  g_signal_connect_swapped (button, "clicked", G_CALLBACK (gtk_widget_destroy), window);
  gtk_container_add (GTK_CONTAINER (button_box), button);

  gtk_widget_show_all (window);
}

int
main (int    argc,
      char **argv)
{
  GtkApplication *app;
  int status;

  app = gtk_application_new ("org.gtk.example", G_APPLICATION_FLAGS_NONE);
  g_signal_connect (app, "activate", G_CALLBACK (activate), NULL);
  status = g_application_run (G_APPLICATION (app), argc, argv);
  g_object_unref (app);

  return status;
}
```

### Упаковка виджетов один в другой

```C
#include <gtk/gtk.h>

static void
print_hello (GtkWidget *widget,
             gpointer   data)
{
  g_print ("Hello World\n");
}

static void
activate (GtkApplication *app,
          gpointer        user_data)
{
  GtkWidget *window;
  GtkWidget *grid;
  GtkWidget *button;

  /* create a new window, and set its title */
  window = gtk_application_window_new (app);
  gtk_window_set_title (GTK_WINDOW (window), "Window");
  gtk_container_set_border_width (GTK_CONTAINER (window), 10);

  /* Here we construct the container that is going pack our buttons */
  grid = gtk_grid_new ();

  /* Pack the container in the window */
  gtk_container_add (GTK_CONTAINER (window), grid);

  button = gtk_button_new_with_label ("Button 1");
  g_signal_connect (button, "clicked", G_CALLBACK (print_hello), NULL);

  /* Place the first button in the grid cell (0, 0), and make it fill
   * just 1 cell horizontally and vertically (ie no spanning)
   */
  gtk_grid_attach (GTK_GRID (grid), button, 0, 0, 1, 1);

  button = gtk_button_new_with_label ("Button 2");
  g_signal_connect (button, "clicked", G_CALLBACK (print_hello), NULL);

  /* Place the second button in the grid cell (1, 0), and make it fill
   * just 1 cell horizontally and vertically (ie no spanning)
   */
  gtk_grid_attach (GTK_GRID (grid), button, 1, 0, 1, 1);

  button = gtk_button_new_with_label ("Quit");
  g_signal_connect_swapped (button, "clicked", G_CALLBACK (gtk_widget_destroy), window);

  /* Place the Quit button in the grid cell (0, 1), and make it
   * span 2 columns.
   */
  gtk_grid_attach (GTK_GRID (grid), button, 0, 1, 2, 1);

  /* Now that we are done packing our widgets, we show them all
   * in one go, by calling gtk_widget_show_all() on the window.
   * This call recursively calls gtk_widget_show() on all widgets
   * that are contained in the window, directly or indirectly.
   */
  gtk_widget_show_all (window);

}

int
main (int    argc,
      char **argv)
{
  GtkApplication *app;
  int status;

  app = gtk_application_new ("org.gtk.example", G_APPLICATION_FLAGS_NONE);
  g_signal_connect (app, "activate", G_CALLBACK (activate), NULL);
  status = g_application_run (G_APPLICATION (app), argc, argv);
  g_object_unref (app);

  return status;
}
```

### Сборка пользовательских интерфейсов.

Интерфейсы можно собирать из XML-файлов.

```C
#include <gtk/gtk.h>

static void
print_hello (GtkWidget *widget,
             gpointer   data)
{
  g_print ("Hello World\n");
}

int
main (int   argc,
      char *argv[])
{
  GtkBuilder *builder;
  GObject *window;
  GObject *button;

  gtk_init (&argc, &argv);

  /* Construct a GtkBuilder instance and load our UI description */
  builder = gtk_builder_new ();
  gtk_builder_add_from_file (builder, "builder.ui", NULL);

  /* Connect signal handlers to the constructed widgets. */
  window = gtk_builder_get_object (builder, "window");
  g_signal_connect (window, "destroy", G_CALLBACK (gtk_main_quit), NULL);

  button = gtk_builder_get_object (builder, "button1");
  g_signal_connect (button, "clicked", G_CALLBACK (print_hello), NULL);

  button = gtk_builder_get_object (builder, "button2");
  g_signal_connect (button, "clicked", G_CALLBACK (print_hello), NULL);

  button = gtk_builder_get_object (builder, "quit");
  g_signal_connect (button, "clicked", G_CALLBACK (gtk_main_quit), NULL);

  gtk_main ();

  return 0;
}
```

Создайте файл с названием builder.ui и следующим содержанием:

```XML
<interface>
  <object id="window" class="GtkWindow">
    <property name="visible">True</property>
    <property name="title">Grid</property>
    <property name="border-width">10</property>
    <child>
      <object id="grid" class="GtkGrid">
        <property name="visible">True</property>
        <child>
          <object id="button1" class="GtkButton">
            <property name="visible">True</property>
            <property name="label">Button 1</property>
          </object>
          <packing>
            <property name="left-attach">0</property>
            <property name="top-attach">0</property>
          </packing>
        </child>
        <child>
          <object id="button2" class="GtkButton">
            <property name="visible">True</property>
            <property name="label">Button 2</property>
          </object>
          <packing>
            <property name="left-attach">1</property>
            <property name="top-attach">0</property>
          </packing>
        </child>
        <child>
          <object id="quit" class="GtkButton">
            <property name="visible">True</property>
            <property name="label">Quit</property>
          </object>
          <packing>
            <property name="left-attach">0</property>
            <property name="top-attach">1</property>
            <property name="width">2</property>
          </packing>
        </child>
      </object>
      <packing>
      </packing>
    </child>
  </object>
</interface>
```

### Создание собственного приложения

Приложение состоит из нескольких файлов:

| Файл | Комментарий |
| - |:-------------:|
| Бинарный файл | Устанавливается в `/usr/bin`. |
| Файл рабочего стола | Этот файл предсотавляет важную информацию о приложении окружению рабочего стола, как например, имя, иконку, имя D-Bus, интерпретатор команд, в котором приложение запускается и так далее. Устанавливается в `/usr/share/application`. |
| Иконка | Иконка устанавливается в `/usr/share/icons/hicolor/48x48/apps`, шже она может быть найдена независимо от текущей темы. |
| Схема настроек | Если приложение использует GSettings, то схема будет установлена в `/usr/share/glib-2.0/shemas`, чтобы инструменты вроде dconf-editor могли ее отыскать |
| Другие ресурсы | Другие файлы, как например UI-файлы GtkBuilder лучше всего загружать из ресурсов, хранимых в самом бинарном файле. Это устраняет необходимость в большинстве файлов, которые традиционно были бы установены в особом для каждого приложения месте внутри `/usr/share`. |

GTK+ включает в себя поддержку приложений, которые являются надстройкой над [GApplication](https://developer.gnome.org/gio/stable/GApplication.html#GApplication-struct). В ходе этого обучения, мы построим простое приложение с нуля, добавляя все больше и больше функций по ходу движения. На этом пути мы столкнемся с [GtkApplication](https://developer.gnome.org/gtk3/stable/GtkApplication.html), шаблонами, ресурсами, меню приложений, настройками, [GtkHeaderBar](https://developer.gnome.org/gtk3/stable/GtkHeaderBar.html), [GtkStack](https://developer.gnome.org/gtk3/stable/GtkStack.html), [GtkSearchBar](https://developer.gnome.org/gtk3/stable/GtkSearchBar.html), [GtkListBox](https://developer.gnome.org/gtk3/stable/GtkListBox.html) и т.д.

Полные, готовые к построению исходники этих примеров могут быть найдены в директории examples/ исходного пакета GTK+ или в [онлайне](https://git.gnome.org/browse/gtk+/tree/examples) в GTK+ git-репозитории. Эти примеры могут быть собраны раздельно, используя файл `Makefile.example`. Для более подробного описания обратитесь к файлу `README` в директории с примерами.

#### Простое приложение

При использовании [GtkApplication](https://developer.gnome.org/gtk3/stable/GtkApplication.html), функция `main()` может быть достаточно простой. Мы просто вызовем `g_application_run()` и передадим экземплляр класса нашего приложения.

```C
#include <gtk/gtk.h>

#include "exampleapp.h"

int
main (int argc, char *argv[])
{
  return g_application_run (G_APPLICATION (example_app_new ()), argc, argv);
}
```

Вся логика приложения будет размещена в классе приложения, который наследует GtkApplication. В нашем примере пока что нет интересного функционала. Все что оно делает - открывает окно, когда запущено без аргументов, и открывает переданные файлы, когда аргументы были переданы.

Для обработки этих двух ситуаций мы перепишем виртуальную функци `activate()`, которая вызывается, когда приложение запускается без аргумаентов и `open()`, которая вызывается, когда приложение запускается с аргументами, переданными посредством командной строк.

```C
#include <gtk/gtk.h>

#include "exampleapp.h"
#include "exampleappwin.h"

struct _ExampleApp
{
  GtkApplication parent;
};

G_DEFINE_TYPE(ExampleApp, example_app, GTK_TYPE_APPLICATION);

static void
example_app_init (ExampleApp *app)
{
}

static void
example_app_activate (GApplication *app)
{
  ExampleAppWindow *win;

  win = example_app_window_new (EXAMPLE_APP (app));
  gtk_window_present (GTK_WINDOW (win));
}

static void
example_app_open (GApplication  *app,
                  GFile        **files,
                  gint           n_files,
                  const gchar   *hint)
{
  GList *windows;
  ExampleAppWindow *win;
  int i;

  windows = gtk_application_get_windows (GTK_APPLICATION (app));
  if (windows)
    win = EXAMPLE_APP_WINDOW (windows->data);
  else
    win = example_app_window_new (EXAMPLE_APP (app));

  for (i = 0; i < n_files; i++)
    example_app_window_open (win, files[i]);

  gtk_window_present (GTK_WINDOW (win));
}

static void
example_app_class_init (ExampleAppClass *class)
{
  G_APPLICATION_CLASS (class)->activate = example_app_activate;
  G_APPLICATION_CLASS (class)->open = example_app_open;
}

ExampleApp *
example_app_new (void)
{
  return g_object_new (EXAMPLE_APP_TYPE,
                       "application-id", "org.gtk.exampleapp",
                       "flags", G_APPLICATION_HANDLES_OPEN,
                       NULL);
}
```

Также заданим иконку и файл рабочего стола

```
[Desktop Entry]
Type=Application
Name=Example
Icon=exampleapp
StartupNotify=true
Exec=@bindir@/exampleapp
```

Обратите внимание, что `@bindir@` нужно заменить на действительнуй путь к бинарному файлу до того, как файл рабочего стола будет использован.

#### Наполнение окна

На этом шаге мы будем использовать шаблон GtkBuilder для ассоциаци UI-файлф с классом окна нашего приложения. Наш UI-файл просто располагает GtkHeaderBar поверх виджета GtkStack. Заголовочна панель содержит GtkStackSwithcer, который является отдельным виджетом для отображения ряда "вкладок" для страниц GtkStack.

```XML
<?xml version="1.0" encoding="UTF-8"?>
<interface>
  <!-- interface-requires gtk+ 3.8 -->
  <template class="ExampleAppWindow" parent="GtkApplicationWindow">
    <property name="title" translatable="yes">Example Application</property>
    <property name="default-width">600</property>
    <property name="default-height">400</property>
    <child>
      <object class="GtkBox" id="content_box">
        <property name="visible">True</property>
        <property name="orientation">vertical</property>
        <child>
          <object class="GtkHeaderBar" id="header">
            <property name="visible">True</property>
            <child type="title">
              <object class="GtkStackSwitcher" id="tabs">
                <property name="visible">True</property>
                <property name="stack">stack</property>
              </object>
            </child>
          </object>
        </child>
        <child>
          <object class="GtkStack" id="stack">
            <property name="visible">True</property>
          </object>
        </child>
      </object>
    </child>
  </template>
</interface>
```


