---
layout: post
title:  "Мой первый пост. Немного тестируем с Capybara"
date:   2022-03-28 02:31:14 +0300
categories: test
---

### Это мой первый экспериментальный пост.

Полчаса назад прочитал про bridgetown в RubyWeekly.
Будем пробовать, будем осваивать, может и на грабельки наступать ;)
Я ещё не решил окончательно, буду ли вести блог вообще...
И следует ли мне остановиться на русском языке.
А может быть предпочесть иной: английский как lingua franca или что-то более мне близкое?
И стилями и оформлением может быть займусь когда-нибудь завтра... Как Алисе предлагалось "варенье на завтра":)
Впрочем, начнём первый пост.

### Jquerry вызывает у меня дрожжь... дрожжь в коленках...

Пару дней назад, ночью в одном из Ruby-чатов в телеграмме у человека возник вопрос и у меня было немного времени и не было настроения заниматься чем то иным...
И ужасное настроение идти спать(
И мы с ним разобрались с его вопросом.
Изначально вопрос был про Capybara и тест, который должен был нажимать на радиобаттон, чтобы обновлять рейтинг, а потом проверять, что рейтинг обновился.
Я по наивности направил в пост на [SO](https://stackoverflow.com/questions/27430074/how-to-click-radio-button-with-capybara-in-ruby-on-rails-app#27432406)

Но потом, в ходе обсуждения в личке с ним, оказалось, что имеет место ajax и jquerry.

### Я их боюсь, потому и дерзаю помочь и разобраться...

Мной было предложено добавить в сценарий теста строки (поэтапно):
```ruby
    Capybara.default_driver = :selenium
    Capybara.ignore_hidden_elements = false
```
И обращаться к видимому элементу с помощью xPath, а не к radio-button.
В результате спрашивающим был написан вот такой рабочий тест:

```ruby
scenario "set rating from user" do
    Capybara.default_driver = :selenium
    Capybara.ignore_hidden_elements = false
    login_as(FactoryBot.create(:user))
    visit movies_path
    click_link "SpiderMan"
    rating = find(".rating > label:nth-child(11)")
    rating.click
    expect(page).to have_content "Rating: 6.0/10"
end
```
Возможно, что что-то можно сделать лучше... Но у нас --- работало!
И глубокая ночь взяла своё. Крепких снов, до новых дерзаний.

----

P.S.: На сделующий день тот же человек написал мне в личку, что тесты не проходят. 
А потом он сам разобрался --- он запускал их с нестабильным интернетом --- и конечно удаленные ссылки на JS-код не загружались.
Оказалось, что он делал тестовое. Удачи в трудоустройстве!

N.B.: Пост написан для себя --- вдруг будет такой же вопрос --- и быстрое решение под боком ;)
А может и тестовое?..

