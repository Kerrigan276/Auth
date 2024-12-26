from django.contrib.auth.models import User
from myapp.models import Author, Category, Post, Comment

user1 = User.objects.create_user('user1')
user2 = User.objects.create_user('user2')

author1 = Author.objects.create(user=user1)
author2 = Author.objects.create(user=user2)

category1 = Category.objects.create(name='Спорт')
category2 = Category.objects.create(name='Политика')
category3 = Category.objects.create(name='Образование')
category4 = Category.objects.create(name='Здоровье')

post1 = Post.objects.create(author=author1, type='статья', title='Первая статья', text='Текст первой статьи...')
post2 = Post.objects.create(author=author2, type='статья', title='Вторая статья', text='Текст второй статьи...')
news1 = Post.objects.create(author=author1, type='новость', title='Первая новость', text='Текст первой новости...')

post1.categories.set([category1, category2])
post2.categories.set([category3])
news1.categories.set([category4, category1])

comment1 = Comment.objects.create(post=post1, user=user2, text='Первый комментарий к первой статье')
comment2 = Comment.objects.create(post=post2, user=user1, text='Первый комментарий ко второй статье')
comment3 = Comment.objects.create(post=news1, user=user2, text='Первый комментарий к новости')
comment4 = Comment.objects.create(post=post1, user=user1, text='Второй комментарий к первой статье')

post1.like()
post1.like()
post2.like()
news1.like()
news1.dislike()
comment1.like()
comment2.dislike()
comment3.like()
comment4.like()

author1.update_rating()
author2.update_rating()

best_author = Author.objects.order_by('-rating').first()
print(f"Лучший пользователь: {best_author.user.username}, рейтинг: {best_author.rating}")

best_post = Post.objects.order_by('-rating').first()
print("Лучшее произведение:")
print(f"Дата: {best_post.created_at}, Автор: {best_post.author.user.username}, Рейтинг: {best_post.rating}")
print(f"Заголовок: {best_post.title}, Превью: {best_post.preview()}")

comments = Comment.objects.filter(post=best_post)
for comment in comments:
    print(f"Дата: {comment.created_at}, Пользователь: {comment.user.username}, Рейтинг: {comment.rating}")
    print(f"Текст: {comment.text}")
