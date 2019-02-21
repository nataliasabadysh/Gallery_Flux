#  Flux - one way data flow

Flux можно применять не только с React, но и с любым другим подходом
визуализации данных, даже с чистым HTML

Flux — это архитектурный паттерн , и у этого паттерна есть реализации . Именно
реализации паттерна Flux и используются разработчиком в реальной разработке.

Action — обычный JavaScript объект, содержащий инструкцию к действию. Чаще
всего — это описание намерения пользователя: логин, переход на другую страницу,
отправка комментария, лайк;

Action creator - Функция возвращает объект с action.


Dispatcher — это синглтон, то есть единая сущность на всё приложение. Его задача
«запускать» действия по однонаправленному потоку данных, передавая их Store;
Именно он и берёт на себя рольуправления действиями (actions).
именно Dispatcher позволяет action пройти дальше по цепочке потока данных Flux. 
вызывая метод Dispatcher.dispatch(action). 


Store — это сущность, содержащая часть состояния приложения. Store'ов в
приложении может быть много. На каждую часть — свой Store. Например: данные о
профиле пользователя и данные о текущем времени могут содержаться в двух
разных Store. Store находится в состоянии прослушивания действий, запущенных
dispatcher'ом;

View — это UI, картинка, отображающая приложение в его текущем состоянии. Об
актуализации отображения UI заботится Store, генерируя события всякий раз, когда
меняется состояние.


# Action:

const showSelectedPhoto = {
    type: 'SHOW_SELECTED_PHOTO',
    payload: 2
};

# Action Creator: 
1)
const showSelectedPhoto = (photoIndex) => {
    return {
        type: 'SHOW_SELECTED_PHOTO',
        payload: photoIndex,
    }
}
showSelectedPhoto(2);

2) Функция login — это action creator, потому что возвращает (creates) объект с action.

const login = (userCredentials) => {
  return {
      type: 'LOG_IN',
      payload: userCredentials,
  }
);

* Использование action creator также открывает доступ к использованию продвинутых
техник по общению с любыми API: Web API, REST API 

3)
const removeUser = (userID) => ({
    type: 'REMOVE_USER',
    payload : userID,
})

4) 
Action
const showSidebar = { type: 'SHOW_SIDEBAR', };

Action Creator
const showSidebarActionCreator = () => { return showSidebar };


# Dispatcher.dispatch(action)

import dispatcher from '../dispatcher';

export default new class PhotoStore {
 constructor () {
    super();
    this.state = {
          photos: [ { id: '1', url: photo1 },{ id: '2', url: photo2 }, { id: '3', url: photo3 }, { id: '4', url: photo4 }
          ],
      selectedPhotoIndex: 0,
  };

 dispatcher.register((action) => {
  switch (action.type) {
      case 'SHOW_NEXT_PHOTO':
      this.showNextPhoto();
      break;
    }
  });
 }
 getAllPhotos () {      return this.state.photos;}
 getSelectedPhoto () {  return this.state.selectedPhoto;}
 getState () {          return this.state; }
 showNextPhoto () {     this.state.selectedPhotoIndex = this.state.selectedPhotoIndex + 1; }
}();



# Flux Standart Action - "FSA"

type      - 
payload   - 
error     - 
meta      -


const removeUserSuccess = userId =>({
  type: 'REMOVE_USER_SUCCESS',
  payload: userId,
})

const removeUserFail = (error, meta) =>({
  type: 'REMOVE_USER_FAIL',
  payload: error,
  meta: meta,
  error: true,
})


const login = () =>({
   type: 'LOGIN',
      payload: {
      email: 'test@email.com',
      password: '12345'
    },
      meta: {
      premiumUser: true
    }
})
## Redux
-  это контейнер для хранения и управлением js програм 
-  Работает с имутаельными структурами данными (Middleware)

# Action — обычный JavaScript объект, 
# Action creator - Функция возвращает объект с action.

# Reducer — это чистая функция. - 
- no side affects
- no mutable 

* Возвращаемое значение reducer — объект состояния.
 const reducer = (state, action) => state 

store.dispatch(action)


export const galleryReducer = (state = initialState, action) => {
 switch (action.type) {
    case SHOW_NEXT_PHOTO:
        if (state.selectedPhotoIndex === state.photos.length - 1) { return state; }
    return state.selectedPhotoIndex: state.selectedPhotoIndex + 1

  default: return state;
 }
};

# combineReducers
B больших приложениях ты будешь располагать множеством reducer'ов. Для реализации
этой задачи Redux предоставляет доступ к функции-утилите combineReducers .

// rootReducer.js
import { combineReducers } from 'redux';
import { galleryReducer } from '../bus/gallery/reducer';
import { usersReducer } from '../bus/users/reducer';

export const rootReducer = combineReducers({
  gallery: galleryReducer,
  users: usersReducer,
})

// store.js
import { createStore } from 'redux';
import { rootReducer } from './rootReducer';

export const store = createStore(rootReducer);

* Таким образом можно объединить сколько угодно reducer'ов в один единый rootReducer,
который впоследствии можно передать функции createStore для создания экземпляра
Store.

* При вызове функции createStore и создании экземпляра Store Redux вызывает все
reducer'ы приложения, зарегистрированные в combineReducers, и использует их
возвращаемые значения для составления объекта с изначальным состоянием
приложения.

*Bсе reducer'ы приложения вызываются на каждый запущенный action

Именно поэтому важно, чтобы каждый reducer приложения имплементировал проверку
на action.type запущенного action, чтобы определить, нужно ли данному reducer менять
состояние в ответ.
Если да — то нужно обновить и вернуть новую, изменённую версию состояния в ответ на
запущенный action.

# No dispatcher in Redux 
# STORE 
- store запускает ation по потоку redux store.dispatch 

# state - obj - содержит всю информацию о приложении 
- state - внутри store 
- для чтения 
- но для изменения 
# Для того что бы изменить состояние можно только одним способом 
      Запустив action по цепочке потока данных Redux
      В конце цепочки action будет передат Reducer на обработку 
      Reducer - имеет право обновлять состояние Redux app


# View Redux 
- получает свежие обновления приложения 
- связан со store 

# И так 

- Запускаеться Action 
-> Store 

Далее запушенный  Action через Store ->  Reducer 

Reducer - проверяет нужно ли обновить состоние на данный Action

-> Обновленное состояние собераеться в rootReduce 
-> Store - обновляет old to new state 
-> new State -> View 



