# Домашнее задание к занятию 11 «Teamcity» - `Иншаков Владимир Александрович`

## Подготовка к выполнению

1. В Yandex Cloud создайте новый инстанс (4CPU4RAM) на основе образа `jetbrains/teamcity-server`.
2. Дождитесь запуска teamcity, выполните первоначальную настройку.
3. Создайте ещё один инстанс (2CPU4RAM) на основе образа `jetbrains/teamcity-agent`. Пропишите к нему переменную окружения `SERVER_URL: "http://<teamcity_url>:8111"`.
4. Авторизуйте агент.
5. Сделайте fork [репозитория](https://github.com/aragastmatb/example-teamcity).
6. Создайте VM (2CPU4RAM) и запустите [playbook](./infrastructure).

## Результаты подготовки

- Виртуальные машины подготовлены:
            https://github.com/MrVanG0gh/Netology_CICD_TeamCity/blob/main/Screens/Screen01.png
![Screen01](https://github.com/MrVanG0gh/Netology_CICD_TeamCity/blob/main/Screens/Screen01.png)

- Сделан форк репозитория:

https://github.com/MrVanG0gh/example-teamcity

- Агент авторизован:

![Screen02](https://github.com/MrVanG0gh/Netology_CICD_TeamCity/blob/main/Screens/Screen02.png)

- Плейбук запущен и Nexus готов к работе:

![Screen03](https://github.com/MrVanG0gh/Netology_CICD_TeamCity/blob/main/Screens/Screen03.png)

## Основная часть

1. Создайте новый проект в teamcity на основе fork.

![Screen11](https://github.com/MrVanG0gh/Netology_CICD_TeamCity/blob/main/Screens/Screen11.png)

2. Сделайте autodetect конфигурации.

![Screen12](https://github.com/MrVanG0gh/Netology_CICD_TeamCity/blob/main/Screens/Screen12.png)
![Screen13](https://github.com/MrVanG0gh/Netology_CICD_TeamCity/blob/main/Screens/Screen13.png)

3. Сохраните необходимые шаги, запустите первую сборку master.

![Screen14](https://github.com/MrVanG0gh/Netology_CICD_TeamCity/blob/main/Screens/Screen14.png)
![Screen15](https://github.com/MrVanG0gh/Netology_CICD_TeamCity/blob/main/Screens/Screen15.png)

4. Поменяйте условия сборки: если сборка по ветке `master`, то должен происходит `mvn clean deploy`, иначе `mvn clean test`.

![Screen16](https://github.com/MrVanG0gh/Netology_CICD_TeamCity/blob/main/Screens/Screen16.png)

5. Для deploy будет необходимо загрузить [settings.xml](./teamcity/settings.xml) в набор конфигураций maven у teamcity, предварительно записав туда креды для подключения к nexus.

![Screen17](https://github.com/MrVanG0gh/Netology_CICD_TeamCity/blob/main/Screens/Screen17.png)

6. В pom.xml необходимо поменять ссылки на репозиторий и nexus.

(+)

7. Запустите сборку по master, убедитесь, что всё прошло успешно и артефакт появился в nexus.

![Screen18](https://github.com/MrVanG0gh/Netology_CICD_TeamCity/blob/main/Screens/Screen18.png)
![Screen19](https://github.com/MrVanG0gh/Netology_CICD_TeamCity/blob/main/Screens/Screen19.png)

8. Мигрируйте `build configuration` в репозиторий.

https://github.com/MrVanG0gh/example-teamcity/tree/master/.teamcity

![Screen20](https://github.com/MrVanG0gh/Netology_CICD_TeamCity/blob/main/Screens/Screen20.png)

9. Создайте отдельную ветку `feature/add_reply` в репозитории.

https://github.com/MrVanG0gh/example-teamcity/tree/feature/add_reply

10. Напишите новый метод для класса Welcomer: метод должен возвращать произвольную реплику, содержащую слово `hunter`.

```
package plaindoll;

public class Welcomer{
	// Если хочешь больше веселья и информации про ДевОпс - приходи в мои каналы NotOps (telegram, YT, Boosty, Patreon)
	// https://t.me/notopsofficial
	public String sayWelcome() {
		return "Welcome home, good hunter. What is it your desire?";
	}
	public String sayFarewell() {
		return "Farewell, good hunter. May you find your worth in waking world.";
	}
	public String sayNeedGold(){
		return "Not enough gold";
	}
	public String saySome(){
		return "something in the way";
	}
	// My new method
	public String sayAJoke(){
	    return "It`s a joke, hunter";
	}
}
```

11. Дополните тест для нового метода на поиск слова `hunter` в новой реплике.

```
package plaindoll;

import static org.hamcrest.CoreMatchers.containsString;
import static org.junit.Assert.*;

import org.junit.Test;

public class WelcomerTest {
	
	private Welcomer welcomer = new Welcomer();
	// Если хочешь больше веселья и информации про ДевОпс - приходи в мои каналы NotOps (telegram, YT, Boosty, Patreon)
	// https://t.me/notopsofficial

	@Test
	public void welcomerSaysWelcome() {
		assertThat(welcomer.sayWelcome(), containsString("Welcome"));
	}
	@Test
	public void welcomerSaysFarewell() {
		assertThat(welcomer.sayFarewell(), containsString("Farewell"));
	}
	@Test
	public void welcomerSaysHunter() {
		assertThat(welcomer.sayWelcome(), containsString("hunter"));
		assertThat(welcomer.sayFarewell(), containsString("hunter"));
	}
	@Test
	public void welcomerSaysSilver(){
		assertThat(welcomer.sayNeedGold(), containsString("gold"));
	}
	@Test
	public void welcomerSaysSomething(){
		assertThat(welcomer.saySome(), containsString("something"));
	}
	//My new test
	@Test
	public void welcomerSaysAJoke(){
	    assertThat(welcomer.sayAJoke(), containsString("hunter"));
	}
}

```

12. Сделайте push всех изменений в новую ветку репозитория.

(+)

13. Убедитесь, что сборка самостоятельно запустилась, тесты прошли успешно.

![Screen21](https://github.com/MrVanG0gh/Netology_CICD_TeamCity/blob/main/Screens/Screen21.png)

14. Внесите изменения из произвольной ветки `feature/add_reply` в `master` через `Merge`.

(+)

15. Убедитесь, что нет собранного артефакта в сборке по ветке `master`.

(+)

16. Настройте конфигурацию так, чтобы она собирала `.jar` в артефакты сборки.

```
target/*.jar => .
```

17. Проведите повторную сборку мастера, убедитесь, что сбора прошла успешно и артефакты собраны.

(+)

18. Проверьте, что конфигурация в репозитории содержит все настройки конфигурации из teamcity.

(+)

19. В ответе пришлите ссылку на репозиторий.

https://github.com/MrVanG0gh/example-teamcity

---
