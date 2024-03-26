<template>
  <div>
    <div class="flex justify-between items-center mt-5">
      <h2 class="text-3xl font-bold mb-8">Заказы</h2>
    </div>

    <div class="grid grid-cols-1 py-4 divide-y bg-white border border-slate-200 rounded-xl p-2">
      <table class="divide-y">
        <thead class="text-lg">
          <tr>
            <th>Заказ</th>
            <th>Дата заказа</th>
            <th>Количество книг</th>
            <th>Дата возврата</th>
            <th>Отметка о возврате</th>
          </tr>
        </thead>
        <tbody class="divide-y text-center font-normal text-gray-500">
          <tr class="cursor-pointer transition hover:-translate-y-2 hover:shadow-xl">
            <td>1</td>
            <td>22.03.2024</td>
            <td>4</td>
            <td>3.04.2024</td>
            <td>Да</td>
          </tr>
          <tr>
            <td>2</td>
            <td>3.04.2024</td>
            <td>6</td>
            <td>12.04.2024</td>
            <td>Да</td>
          </tr>
          <tr>
            <td>3</td>
            <td>12.04.2024</td>
            <td>2</td>
            <td>24.04.2024</td>
            <td>Нет</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
