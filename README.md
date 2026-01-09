# Flutter BLoC (Business Logic Component) – Complete Beginner to Advanced Guide

This README explains **BLoC in Flutter** step by step, in simple words, from **beginner to advanced**.

---

## 1. What is BLoC?

**BLoC = Business Logic Component**

BLoC is a **state management pattern** that separates:

* **UI (presentation)**
* **Business logic**

### Simple Flow

```
UI → Event → BLoC → State → UI
```

* UI sends **Events**
* BLoC processes logic
* BLoC emits **States**
* UI rebuilds based on state

---

## 2. Why use BLoC?

Problems without BLoC:

* Logic mixed with UI
* Hard to test
* Difficult to scale

Benefits of BLoC:

* Clean architecture
* Easy testing
* Predictable state flow
* Good for large apps

---

## 3. Core Concepts (Very Important)

### 3.1 Event

Event represents **user actions** or **triggers**.

Examples:

* Button click
* API request
* Refresh action

```dart
abstract class CounterEvent {}

class Increment extends CounterEvent {}
class Decrement extends CounterEvent {}
```

---

### 3.2 State

State represents **UI condition**.

```dart
class CounterState {
  final int count;
  CounterState(this.count);
}
```

---

### 3.3 BLoC

BLoC receives events and emits states.

```dart
class CounterBloc extends Bloc<CounterEvent, CounterState> {
  CounterBloc() : super(CounterState(0)) {

    on<Increment>((event, emit) {
      emit(CounterState(state.count + 1));
    });

    on<Decrement>((event, emit) {
      emit(CounterState(state.count - 1));
    });
  }
}
```

---

## 4. Data Flow (Must Understand)

```
User Action
   ↓
Event added
   ↓
BLoC handles logic
   ↓
New State emitted
   ↓
UI rebuilds automatically
```

UI **never changes data directly**.

---

## 5. Add Dependencies

```yaml
dependencies:
  flutter_bloc: ^8.1.3
```

---

## 6. Using BLoC in UI

### 6.1 Provide BLoC

```dart
BlocProvider(
  create: (_) => CounterBloc(),
  child: CounterScreen(),
);
```

---

### 6.2 Read State (BlocBuilder)

```dart
BlocBuilder<CounterBloc, CounterState>(
  builder: (context, state) {
    return Text(
      state.count.toString(),
      style: TextStyle(fontSize: 40),
    );
  },
);
```

---

### 6.3 Send Event

```dart
FloatingActionButton(
  onPressed: () {
    context.read<CounterBloc>().add(Increment());
  },
  child: Icon(Icons.add),
);
```

---

## 7. BlocBuilder vs BlocListener

### BlocBuilder

* Used to rebuild UI

```dart
BlocBuilder<MyBloc, MyState>(...)
```

### BlocListener

* One-time actions (Snackbar, Dialog, Navigation)

```dart
BlocListener<MyBloc, MyState>(
  listener: (context, state) {
    if (state is ErrorState) {
      ScaffoldMessenger.of(context)
          .showSnackBar(SnackBar(content: Text("Error")));
    }
  },
);
```

---

## 8. BlocConsumer

Use when you need **both UI rebuild and side effects**.

```dart
BlocConsumer<MyBloc, MyState>(
  listener: (context, state) {},
  builder: (context, state) {},
);
```

---

## 9. Common API State Pattern

```dart
abstract class ApiState {}

class Loading extends ApiState {}

class Success extends ApiState {
  final List data;
  Success(this.data);
}

class Error extends ApiState {
  final String message;
  Error(this.message);
}
```

---

## 10. Cubit vs BLoC

| Feature    | Cubit | BLoC   |
| ---------- | ----- | ------ |
| Events     | ❌     | ✅      |
| Simplicity | Easy  | Medium |
| Structure  | Loose | Strict |

**Cubit** is recommended for small logic.

---

## 11. Folder Structure (Recommended)

```
lib/
 ├── bloc/
 │    ├── counter_bloc.dart
 │    ├── counter_event.dart
 │    └── counter_state.dart
 ├── ui/
 │    └── counter_screen.dart
```

---

## 12. When to Use BLoC?

Use BLoC when:

* Medium to large apps
* API + sockets
* Team projects
* Clean architecture required

Avoid when:

* Very small apps

---

## 13. BLoC vs Provider vs Riverpod

| Feature    | Provider | BLoC   | Riverpod |
| ---------- | -------- | ------ | -------- |
| Learning   | Easy     | Medium | Medium   |
| Structure  | Loose    | Strict | Flexible |
| Large Apps | ❌        | ✅      | ✅        |
| Testable   | ⚠️       | ✅      | ✅        |

---

## 14. Real-Life Mapping

| Feature | Event        | State                     |
| ------- | ------------ | ------------------------- |
| Login   | LoginPressed | Loading / Success / Error |
| API     | FetchData    | Loading / Success / Error |
| Cart    | AddItem      | UpdatedCart               |

---

### Author

**Shravan Kushwaha**

Happy Flutter Coding 🚀
