<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Koomek — Услуги по всему Казахстану</title>
  <style>
    body { font-family: sans-serif; background: #f4f4f4; margin: 0; padding: 0; }
    header { background: #2c3e50; color: white; padding: 1rem; text-align: center; }
    .container { max-width: 800px; margin: 2rem auto; background: white; padding: 1rem; border-radius: 8px; }
    h2 { color: #2c3e50; }
    .announcement { border-bottom: 1px solid #ccc; padding: 1rem 0; }
    label { display: block; margin-top: 1rem; }
    input, select, textarea { width: 100%; padding: 0.5rem; margin-top: 0.25rem; }
    button { margin-top: 1rem; padding: 0.7rem 1.5rem; background: #3498db; color: white; border: none; border-radius: 5px; cursor: pointer; }
    button:hover { background: #2980b9; }
    .filters { display: flex; gap: 1rem; margin-bottom: 1rem; }
  </style>
</head>
<body>
  <header>
    <h1>Koomek — Платформа услуг</h1>
    <p>Находите или размещайте услуги по всему Казахстану</p>
  </header>
  <div class="container">
    <h2>Добавить объявление</h2>
    <form id="postForm">
      <label>Категория:
        <select name="category">
          <option>Ремонт</option>
          <option>Обучение</option>
          <option>Доставка</option>
          <option>Дизайн</option>
          <option>Услуги по дому</option>
          <option>Курьер / такси</option>
          <option>Красота и здоровье</option>
          <option>IT и цифровые услуги</option>
          <option>Прочее</option>
        </select>
      </label>
      <label>Город:
        <input type="text" name="city" placeholder="Например, Алматы" required />
      </label>
      <label>Описание:
        <textarea name="description" rows="4" placeholder="Что вы предлагаете или ищете?" required></textarea>
      </label>
      <label>Контакты:
        <input type="text" name="contact" placeholder="Telegram, WhatsApp, номер телефона" required />
      </label>
      <button type="submit">Разместить</button>
    </form>

    <h2>Объявления</h2>
    <div class="filters">
      <select id="filterCategory">
        <option value="">Все категории</option>
        <option>Ремонт</option>
        <option>Обучение</option>
        <option>Доставка</option>
        <option>Дизайн</option>
        <option>Услуги по дому</option>
        <option>Курьер / такси</option>
        <option>Красота и здоровье</option>
        <option>IT и цифровые услуги</option>
        <option>Прочее</option>
      </select>
      <input type="text" id="filterCity" placeholder="Город" />
    </div>
    <div id="announcements"></div>
  </div>

  <script>
    const form = document.getElementById('postForm');
    const announcements = document.getElementById('announcements');
    const filterCategory = document.getElementById('filterCategory');
    const filterCity = document.getElementById('filterCity');

    let posts = [];

    form.onsubmit = (e) => {
      e.preventDefault();
      const data = new FormData(form);
      const post = {
        category: data.get('category'),
        city: data.get('city'),
        description: data.get('description'),
        contact: data.get('contact')
      };
      posts.unshift(post);
      form.reset();
      renderPosts();
    };

    filterCategory.oninput = renderPosts;
    filterCity.oninput = renderPosts;

    function renderPosts() {
      announcements.innerHTML = '';
      const category = filterCategory.value;
      const city = filterCity.value.toLowerCase();
      posts.filter(p => {
        return (!category || p.category === category) && (!city || p.city.toLowerCase().includes(city));
      }).forEach(p => {
        const el = document.createElement('div');
        el.className = 'announcement';
        el.innerHTML = `<strong>${p.category} — ${p.city}</strong><br>${p.description}<br><em>${p.contact}</em>`;
        announcements.appendChild(el);
      });
    }
  </script>
</body>
</html>
