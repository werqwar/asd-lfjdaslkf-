## Оглавление

1. [Добавить новый тип челленджа "multiple_of" (Вероятность: 90%)](#1-добавить-новый-тип-челленджа-multiple_of-вероятность-90)
2. [Добавить новый тип челленджа "close_to" (Вероятность: 85%)](#2-добавить-новый-тип-челленджа-close_to-вероятность-85)
3. [Исправить drag-and-drop: ограничить только Y-ось (Вероятность: 85%)](#3-исправить-drag-and-drop-ограничить-только-y-ось-вероятность-85)
4. [Добавить визуальную индикацию прогресса (Вероятность: 75%)](#4-добавить-визуальную-индикацию-прогресса-вероятность-75)
5. [Улучшить обработку ошибок (визуальное отображение) (Вероятность: 65%)](#5-улучшить-обработку-ошибок-визуальное-отображение-вероятность-65)
6. [Подсвечивать точки, выходящие за диапазон (Вероятность: 62%)](#6-подсвечивать-точки-выходящие-за-диапазон-вероятность-62)
7. [Добавить счётчик попыток (Вероятность: 57%)](#7-добавить-счётчик-попыток-вероятность-57)
8. [Добавить кнопку "Hint" / Подсказки (Вероятность: 55%)](#8-добавить-кнопку-hint--подсказки-вероятность-55)
9. [Добавить отображение координат под графиком (Вероятность: 52%)](#9-добавить-отображение-координат-под-графиком-вероятность-52)
10. [Добавить Undo/Redo функциональность (Вероятность: 50%)](#10-добавить-undoredo-функциональность-вероятность-50)
11. [Добавить анимацию при успехе (Вероятность: 45%)](#11-добавить-анимацию-при-успехе-вероятность-45)
12. [Добавить точку кликом по графику (Вероятность: 45%)](#12-добавить-точку-кликом-по-графику-вероятность-45)
13. [Экспорт статистики в JSON (кнопка → файл) (Вероятность: 42%)](#13-экспорт-статистики-в-json-кнопка--файл-вероятность-42)
14. [Добавить удаление точек (Вероятность: 40%)](#14-добавить-удаление-точек-вероятность-40)
15. [Добавить фильтрацию челленджей по типу (Вероятность: 40%)](#15-добавить-фильтрацию-челленджей-по-типу-вероятность-40)
16. [Добавить endpoint для истории попыток (Вероятность: 35%)](#16-добавить-endpoint-для-истории-попыток-вероятность-35)
17. [Ограничить перемещение точек только по X (Вероятность: 32%)](#17-ограничить-перемещение-точек-только-по-x-вероятность-32)
18. [Добавить звуковые эффекты (Вероятность: 30%)](#18-добавить-звуковые-эффекты-вероятность-30)
19. [Добавить баг: при клике снова на сабмит при любом фидбеке в статистику в тотал добавляется 1 (Вероятность: 50%)](#19-добавить-баг-при-клике-снова-на-сабмит-при-любом-фидбеке-в-статистику-в-тотал-добавляется-1-вероятность-50)
20. [Авторизация простая через JWT (Вероятность: 60%)](#20-авторизация-простая-через-jwt-вероятность-60)
21. [Новый тип челленджа "average_y" (Вероятность: 70%)](#21-новый-тип-челленджа-average_y-вероятность-70)
22. [Новый тип челленджа "sum_in_range" (Вероятность: 70%)](#22-новый-тип-челленджа-sum_in_range-вероятность-70)
23. [Добавить фильтры в /history (Вероятность: 45%)](#23-добавить-фильтры-в-history-вероятность-45)
24. [Переключатель темы светлая/темная](#24-переключатель-темы-светлая-темная)
25. [Среднее время решения](#25-среднее-время-решения)
26. [Индикатор прогресса (процент)](#26-индикатор-прогресса-процент)
27. [Градиентная заливка между min и max](#27-градиентная-заливка-между-min-и-max)
28. [Несколько наборов данных](#28-несколько-наборов-данных)

---

## 1. Добавить новый тип челленджа "multiple_of" (Вероятность: 90%)

### Описание
Добавить новый тип челленджа, который требует, чтобы range был кратен указанному числу (например, кратен 50).

**Примечание**: `exact` проверяет точные min/max значения, а `multiple_of` проверяет только range (разницу), что делает его концептуально отличным.

### Решение

#### Шаг 1: Обновить `backend/api/challenges.py`
Добавить новые челленджи в массив `CHALLENGE_TYPES`:

```python
{"type": "multiple_of", "value": 50, "description": "Make the range a multiple of 50"},
{"type": "multiple_of", "value": 100, "description": "Make the range a multiple of 100"},
{"type": "multiple_of", "value": 25, "description": "Make the range a multiple of 25"},
```

#### Шаг 2: Обновить `backend/api/views.py`
Добавить логику валидации в функцию `validate_range`:

```python
elif challenge_type == "multiple_of":
    divisor = challenge.get('value', 50)
    is_correct = current_range % divisor == 0
    feedback = f"Your range is {current_range:.0f}. Target: multiple of {divisor}. {'✓ Correct!' if is_correct else '✗ Try again!'}"
```

#### Шаг 3: Обновить `frontend/src/components/Tutor.jsx`
Добавить отображение нового типа челленджа:

```jsx
{challenge.type === 'multiple_of' && (
  <span>Target: Range must be a multiple of {challenge.value}</span>
)}
```

### Время выполнения: 15-20 минут

---

## 2. Добавить новый тип челленджа "close_to" (Вероятность: 85%)

### Описание
Добавить новый тип челленджа, который требует, чтобы range был близок к указанному значению (например, ±10).

**Разница с `exact`**: 
- `exact` проверяет min и max значения (границы диапазона) с допуском ±10
- `close_to` проверяет только range (разницу), не важно какие min/max

**Пример**: 
- `exact` min=150, max=400 → нужно min в диапазоне 140-160 И max в диапазоне 390-410
- `close_to` value=300 → нужно range≈300 (может быть min=100, max=400 или min=200, max=500)

### Решение

#### Шаг 1: Обновить `backend/api/challenges.py`
Добавить новые челленджи в массив `CHALLENGE_TYPES`:

```python
{"type": "close_to", "value": 300, "tolerance": 10, "description": "Make the range close to 300 (±10)"},
{"type": "close_to", "value": 250, "tolerance": 15, "description": "Make the range close to 250 (±15)"},
{"type": "close_to", "value": 350, "tolerance": 20, "description": "Make the range close to 350 (±20)"},
```

#### Шаг 2: Обновить `backend/api/views.py`
Добавить логику валидации в функцию `validate_range`:

```python
elif challenge_type == "close_to":
    target = challenge.get('value', 300)
    tolerance = challenge.get('tolerance', 10)
    is_correct = abs(current_range - target) <= tolerance
    feedback = f"Your range is {current_range:.0f}. Target: close to {target} (±{tolerance}). {'✓ Correct!' if is_correct else '✗ Try again!'}"
```

#### Шаг 3: Обновить `frontend/src/components/Tutor.jsx`
Добавить отображение нового типа челленджа:

```jsx
{challenge.type === 'close_to' && (
  <span>Target: Range close to {challenge.value} (±{challenge.tolerance})</span>
)}
```

### Время выполнения: 15-20 минут

---

## 3. Исправить drag-and-drop: ограничить только Y-ось (Вероятность: 85%)

### Описание
Сейчас можно двигать пузырьки по обеим осям (X и Y), но по инструкции нужно только по Y-оси.

### Решение

#### Обновить `frontend/src/components/BubbleGraph.jsx`
Изменить функцию `handleMouseMove` (строки 47-75):

```jsx
const handleMouseMove = useCallback((e) => {
  if (!draggedPoint) return;
  
  const bounds = getChartBounds();
  if (!bounds) return;
  
  // Удаляем deltaX - не используем движение по X
  const deltaY = e.clientY - dragStart.y;
  
  // Удаляем вычисление xDelta
  const yDelta = -(deltaY / bounds.height) * (graphData.yAxis.max - graphData.yAxis.min);
  
  // Оставляем X без изменений
  const newX = dragStart.pointX; // Не изменяем X
  
  const newY = Math.max(
    graphData.yAxis.min,
    Math.min(graphData.yAxis.max, dragStart.pointY + yDelta)
  );
  
  const updatedPoints = points.map((p) =>
    p.id === draggedPoint
      ? { ...p, x: Math.round(newX), y: Math.round(newY) }
      : p
  );
  
  onPointUpdate(updatedPoints);
}, [draggedPoint, dragStart, graphData, getChartBounds, points, onPointUpdate]);
```

### Время выполнения: 10-15 минут

---

## 4. Добавить визуальную индикацию прогресса (Вероятность: 75%)

### Описание
Добавить прогресс-бар, показывающий, насколько близко пользователь к цели.

### Решение

#### Шаг 1: Создать компонент `frontend/src/components/ProgressBar.jsx`

```jsx
function ProgressBar({ currentRange, challenge, graphData }) {
  if (!challenge) return null;

  const calculateProgress = () => {
    const { type, value, min, max, tolerance } = challenge;
    const range = Number(currentRange.range);
    const numValue = Number(value);
    let progress = 0;

    const maxPossibleRange = graphData?.yAxis
      ? graphData.yAxis.max - graphData.yAxis.min
      : 450;

    if (type === 'less_than') {
      if (range < numValue) {
        progress = 100;
      } else {
        progress = Math.max(0, Math.min(99, ((numValue / range) * 100)));
      }
    }
    else if (type === 'greater_than') {
      if (range > numValue) {
        progress = 100;
      } else {
        progress = Math.max(0, Math.min(99, (range / numValue) * 100));
      }
    }
    else if (type === 'between') {
      if (range >= min && range <= max) {
        progress = 100;
      } else if (range < min) {
        progress = Math.max(0, Math.min(50, (range / min) * 50));
      } else {
        const overshoot = range - max;
        const maxOvershoot = maxPossibleRange - max;
        if (maxOvershoot > 0) {
          progress = Math.max(0, Math.min(50, 50 - (overshoot / maxOvershoot) * 50));
        } else {
          progress = 0;
        }
      }
    }
    else if (type === 'exact') {
      const targetMin = min;
      const targetMax = max;
      const minDiff = Math.abs(currentRange.min - targetMin);
      const maxDiff = Math.abs(currentRange.max - targetMax);
      const exactTolerance = 10;

      if (minDiff <= exactTolerance && maxDiff <= exactTolerance) {
        progress = 100;
      } else {
        const minProgress = Math.max(0, 100 - (minDiff / exactTolerance) * 50);
        const maxProgress = Math.max(0, 100 - (maxDiff / exactTolerance) * 50);
        progress = (minProgress + maxProgress) / 2;
      }
    }
    else if (type === 'close_to') {
      const distance = Math.abs(range - value);
      const tol = tolerance || 10;

      if (distance <= tol) {
        progress = 100;
      } else {
        const maxDistance = Math.max(tol, maxPossibleRange);
        progress = Math.max(0, 100 - ((distance - tol) / (maxDistance - tol)) * 100);
      }
    }
    else if (type === 'multiple_of') {
      const remainder = range % value;
      if (remainder === 0) {
        progress = 100;
      } else {
        progress = Math.max(0, 100 - (remainder / value) * 100);
      }
    }

    return Math.round(progress);
  };

  const progress = calculateProgress();

  return (
    <div >
      <div>
        Progress: {progress}%
      </div>
      <div>
        <div
          style={{ width: `${progress}%` }}
        />
      </div>
    </div>
  );
}

export default ProgressBar;
```

#### Шаг 2: Обновить `frontend/src/App.jsx`
Импортировать и использовать компонент:

```jsx
import ProgressBar from './components/ProgressBar';

// В компоненте App, добавить после BubbleGraph:
{graphData && challenge && points.length > 0 && (
        <ProgressBar
            currentRange={{
            min: Math.min(...points.map(p => p.y)),
            max: Math.max(...points.map(p => p.y)),
            range: Math.max(...points.map(p => p.y)) - Math.min(...points.map(p => p.y))
            }}
            challenge={challenge}
            graphData={graphData}
        />
        )}
```

### Время выполнения: 20-25 минут

---

## 5. Улучшить обработку ошибок (визуальное отображение) (Вероятность: 65%)

### Описание
Сейчас ошибки только в консоли. Нужно показывать пользователю понятные сообщения.

### Решение

#### Шаг 1: Обновить `frontend/src/App.jsx`
Добавить состояние для ошибок:

```jsx
const [error, setError] = useState(null);

// Обновить функции загрузки:
const loadInitialData = async () => {
  try {
    setError(null);
    const response = await axios.get(`${API_BASE_URL}/data/`);
    setGraphData(response.data);
    const initial = response.data.points;
    setInitialPoints(initial);
    setPoints(initial);
    setLoading(false);
  } catch (error) {
    console.error('Error loading data:', error);
    setError('Failed to load graph data. Please refresh the page.');
    setLoading(false);
  }
};

const loadChallenge = async () => {
  try {
    setError(null);
    const response = await axios.get(`${API_BASE_URL}/challenge/`);
    setChallenge(response.data);
    setFeedback(null);
  } catch (error) {
    console.error('Error loading challenge:', error);
    setError('Failed to load challenge. Please try again.');
  }
};

const handleSubmit = async () => {
  try {
    setError(null);
    const response = await axios.post(`${API_BASE_URL}/validate/`, {
      points: points,
      challenge: challenge,
    });
    setFeedback(response.data);
    // ... остальной код статистики
  } catch (error) {
    console.error('Error validating:', error);
    setError('Failed to validate your answer. Please check your connection and try again.');
    setFeedback({
      is_correct: false,
      feedback: 'Error validating your answer. Please try again.',
    });
  }
};

// В JSX, добавить перед controls:
{error && (
  <div className="error-message" style={{
    background: '#ffebee',
    color: '#c62828',
    padding: '15px',
    borderRadius: '8px',
    marginBottom: '20px',
    borderLeft: '4px solid #f44336'
  }}>
    <strong>⚠️ Error:</strong> {error}
    <button 
      onClick={() => setError(null)}
      style={{
        float: 'right',
        background: 'transparent',
        border: 'none',
        fontSize: '18px',
        cursor: 'pointer'
      }}
    >
      ×
    </button>
  </div>
)}
```

### Время выполнения: 15-20 минут

---

## 6. Подсвечивать точки, выходящие за диапазон (Вероятность: 62%)

### Описание
Визуально выделять точки, которые находятся вне целевого диапазона для текущего челленджа. Это помогает пользователю понять, какие точки нужно переместить.

**Важно:** Решение работает для всех типов челленджей:
- `exact` - подсвечивает точки, которые выходят за целевой диапазон (min-10, max+10)
- `less_than` - подсвечивает крайние точки (min и max), если range >= value (нужно уменьшить range)
- `greater_than` - подсвечивает крайние точки (min и max), если range <= value (нужно увеличить range)
- `between` - подсвечивает крайние точки, если range вне диапазона [min, max]
- `multiple_of` - подсвечивает крайние точки, если range не кратен value
- `close_to` - подсвечивает крайние точки, если range не близок к value (с учетом tolerance)
- `average_y` - подсвечивает точки, которые нужно изменить для достижения целевого среднего
- `sum_in_range` - подсвечивает точки, которые нужно изменить для достижения целевой суммы

**Логика работы:**
- Для челленджей на основе range (less_than, greater_than, between, multiple_of, close_to) - подсвечиваются крайние точки (min и max), так как именно их перемещение влияет на range
- Для челленджей на основе average_y - подсвечиваются точки, которые нужно поднять или опустить для достижения целевого среднего
- Для челленджей на основе sum_in_range - подсвечиваются точки, которые нужно изменить для достижения целевой суммы

### Решение

#### Шаг 1: Обновить `frontend/src/components/BubbleGraph.jsx`
Добавить логику определения точек, выходящих за диапазон, и визуальное выделение:

```jsx
// Добавить функцию для определения, выходит ли точка за диапазон
const isPointOutOfRange = useCallback((point, challenge) => {
  if (!challenge) return false;
  
  const yValues = points.map(p => p.y);
  if (yValues.length === 0) return false;
  
  const currentMin = Math.min(...yValues);
  const currentMax = Math.max(...yValues);
  const currentRange = currentMax - currentMin;
  const averageY = yValues.reduce((sum, y) => sum + y, 0) / yValues.length;
  const sumY = yValues.reduce((sum, y) => sum + y, 0);
  
  const { type, value, min, max, tolerance } = challenge;
  
  if (type === 'exact') {
    const targetMin = min;
    const targetMax = max;
    // Точка выходит за диапазон, если она ниже min-10 или выше max+10
    return point.y < targetMin - 10 || point.y > targetMax + 10;
  } 
  else if (type === 'less_than') {
    // Если range >= value, нужно уменьшить range - подсвечиваем крайние точки
    if (currentRange >= value) {
      return point.y === currentMin || point.y === currentMax;
    }
    return false;
  } 
  else if (type === 'greater_than') {
    // Если range <= value, нужно увеличить range - подсвечиваем крайние точки
    // чтобы показать, что их нужно раздвинуть дальше
    if (currentRange <= value) {
      return point.y === currentMin || point.y === currentMax;
    }
    return false;
  } 
  else if (type === 'between') {
    // Если range вне диапазона, подсвечиваем крайние точки
    if (currentRange < min || currentRange > max) {
      return point.y === currentMin || point.y === currentMax;
    }
    return false;
  }
  else if (type === 'multiple_of') {
    // Если range не кратен value, подсвечиваем крайние точки
    if (currentRange % value !== 0) {
      return point.y === currentMin || point.y === currentMax;
    }
    return false;
  }
  else if (type === 'close_to') {
    // Если range не близок к value, подсвечиваем крайние точки
    const target = value;
    const tol = tolerance || 10;
    if (Math.abs(currentRange - target) > tol) {
      return point.y === currentMin || point.y === currentMax;
    }
    return false;
  }
  else if (type === 'average_y') {
    // Подсвечиваем точки, которые нужно изменить для достижения целевого среднего
    const targetMin = min;
    const targetMax = max;
    const targetAverage = (targetMin + targetMax) / 2;
    
    if (averageY < targetMin || averageY > targetMax) {
      // Определяем, какие точки нужно изменить
      if (averageY < targetMin) {
        // Среднее слишком низкое - нужно поднять точки
        // Подсвечиваем точки, которые ниже целевого среднего (их нужно поднять)
        return point.y < targetAverage;
      } else {
        // Среднее слишком высокое - нужно опустить точки
        // Подсвечиваем точки, которые выше целевого среднего (их нужно опустить)
        return point.y > targetAverage;
      }
    }
    return false;
  }
  else if (type === 'sum_in_range') {
    // Подсвечиваем точки, которые нужно изменить для достижения целевой суммы
    const targetMin = min;
    const targetMax = max;
    const targetSum = (targetMin + targetMax) / 2;
    
    if (sumY < targetMin || sumY > targetMax) {
      // Определяем, какие точки нужно изменить
      if (sumY < targetMin) {
        // Сумма слишком маленькая - нужно увеличить значения
        // Подсвечиваем точки, которые ниже среднего (их нужно поднять)
        return point.y < averageY;
      } else {
        // Сумма слишком большая - нужно уменьшить значения
        // Подсвечиваем точки, которые выше среднего (их нужно опустить)
        return point.y > averageY;
      }
    }
    return false;
  }
  
  return false;
}, [points]);

// Обновить renderCustomShape для подсветки:
const renderCustomShape = useCallback((props) => {
  const { cx, cy, payload, z } = props;
  if (cx == null || cy == null) return null;
  
  const radius = z / 2;
  const pointId = payload.id;
  const isDragged = draggedPoint === pointId;
  
  const point = points.find(p => p.id === pointId);
  if (!point) return null;
  
  // Проверяем, выходит ли точка за диапазон
  const outOfRange = challenge ? isPointOutOfRange(point, challenge) : false;
  
  return (
    <circle
      cx={cx}
      cy={cy}
      r={radius}
      fill={isDragged ? '#764ba2' : (outOfRange ? '#ff6b6b' : '#667eea')}
      stroke={outOfRange ? '#c92a2a' : '#fff'}
      strokeWidth={outOfRange ? 3 : 2}
      style={{ cursor: 'pointer' }}
      onMouseDown={(e) => {
        e.preventDefault();
        e.stopPropagation();
        handleBubbleMouseDown(e, point);
      }}
    />
  );
}, [draggedPoint, points, handleBubbleMouseDown, challenge, isPointOutOfRange]);
```

#### Шаг 2: Передать challenge в BubbleGraph
Обновить `frontend/src/App.jsx`:

```jsx
<BubbleGraph
  graphData={graphData}
  points={points}
  onPointUpdate={handlePointUpdate}
  challenge={challenge}
/>
```

#### Шаг 3: Обновить пропсы BubbleGraph
В `frontend/src/components/BubbleGraph.jsx`:

```jsx
function BubbleGraph({ graphData, points, onPointUpdate, challenge }) {
  // ... остальной код
}
```

**Важные замечания:**

1. **Проверка на пустой массив точек:** Добавлена проверка `if (yValues.length === 0) return false;` для избежания ошибок при пустом массиве.

2. **Для `greater_than`:** Изменена логика - теперь подсвечиваются только крайние точки (min и max), а не все точки, так как именно их перемещение влияет на range.

3. **Для `average_y`:** Используется целевое среднее `(targetMin + targetMax) / 2` вместо текущего среднего для более точного определения, какие точки нужно изменить.

4. **Для `sum_in_range`:** Используется текущее среднее для определения, какие точки нужно изменить (те, что выше/ниже среднего).

5. **Зависимости useCallback:** Убедитесь, что `isPointOutOfRange` имеет правильные зависимости `[points]`, чтобы функция пересчитывалась при изменении точек.

**Отладка:**
Если подсветка не работает, проверьте:
- Передается ли `challenge` в компонент `BubbleGraph`
- Правильно ли определяется тип челленджа (`challenge.type`)
- Корректно ли вычисляются `currentMin`, `currentMax`, `currentRange`
- Работает ли функция `isPointOutOfRange` для конкретного типа челленджа

### Время выполнения: 15-20 минут

---

## 7. Добавить счётчик попыток (Вероятность: 57%)

### Описание
Добавить счётчик, показывающий количество попыток для текущего челленджа. Сбрасывается при загрузке нового челленджа.

### Решение

#### Обновить `frontend/src/App.jsx`

```jsx
const [attemptsCount, setAttemptsCount] = useState(0);

// Обновить handleSubmit:
const handleSubmit = async () => {
  try {
    const response = await axios.post(`${API_BASE_URL}/validate/`, {
      points: points,
      challenge: challenge,
    });
    setFeedback(response.data);
    setAttemptsCount(prev => prev + 1);
    
    const newStats = {
      ...statistics,
      total: statistics.total + 1,
      completed: response.data.is_correct ? statistics.completed + 1 : statistics.completed,
    };
    newStats.successRate = newStats.total > 0 
      ? Math.round((newStats.completed / newStats.total) * 100) 
      : 0;
    setStatistics(newStats);
    localStorage.setItem('rangeAppStatistics', JSON.stringify(newStats));
  } catch (error) {
    console.error('Error validating:', error);
    setFeedback({
      is_correct: false,
      feedback: 'Error validating your answer. Please try again.',
    });
  }
};

// Обновить loadChallenge для сброса счётчика:
const loadChallenge = async () => {
  try {
    const response = await axios.get(`${API_BASE_URL}/challenge/`);
    setChallenge(response.data);
    setFeedback(null);
    setAttemptsCount(0); // Сброс счётчика при новом челлендже
  } catch (error) {
    console.error('Error loading challenge:', error);
  }
};

// В JSX, добавить счётчик попыток (например, в Tutor или рядом с challenge):
{challenge && (
  <div style={{
    marginBottom: '10px',
    padding: '10px',
    background: '#f8f9fa',
    borderRadius: '6px',
    textAlign: 'center'
  }}>
    <span style={{ fontWeight: 600, color: '#666' }}>
      Attempts: <span style={{ color: '#667eea' }}>{attemptsCount}</span>
    </span>
  </div>
)}
```

### Время выполнения: 10-15 минут

---

## 8. Добавить кнопку "Hint" / Подсказки (Вероятность: 55%)

### Описание
Добавить кнопку "Hint", которая показывает подсказку, как достичь цели.

### Решение

#### Шаг 1: Обновить `frontend/src/components/Tutor.jsx`

```jsx
import React, { useState } from 'react';
import styles from './Tutor.module.css';

function Tutor({ challenge, points }) {
  const [showHint, setShowHint] = useState(false);

  const getHint = () => {
    if (!challenge || !points) return '';

    const yValues = points.map(p => p.y);
    const currentMin = Math.min(...yValues);
    const currentMax = Math.max(...yValues);
    const currentRange = currentMax - currentMin;

    const { type, value, min, max } = challenge;

    if (type === 'less_than') {
      if (currentRange >= value) {
        return `Your range is ${currentRange}, but you need less than ${value}. Try moving some points closer together on the y-axis.`;
      }
      return `Great! Your range is ${currentRange}, which is less than ${value}. You're on the right track!`;
    } else if (type === 'greater_than') {
      if (currentRange <= value) {
        return `Your range is ${currentRange}, but you need greater than ${value}. Try moving points further apart on the y-axis.`;
      }
      return `Perfect! Your range is ${currentRange}, which is greater than ${value}. Well done!`;
    } else if (type === 'between') {
      if (currentRange < min) {
        return `Your range is ${currentRange}, but you need between ${min} and ${max}. Move points further apart.`;
      } else if (currentRange > max) {
        return `Your range is ${currentRange}, but you need between ${min} and ${max}. Move points closer together.`;
      }
      return `Excellent! Your range is ${currentRange}, which is between ${min} and ${max}.`;
    } else if (type === 'exact') {
      const targetMin = min;
      const targetMax = max;
      if (currentMin < targetMin - 10) {
        return `Your minimum is ${currentMin}, but you need ${targetMin}. Move the lowest point up.`;
      } else if (currentMin > targetMin + 10) {
        return `Your minimum is ${currentMin}, but you need ${targetMin}. Move the lowest point down.`;
      } else if (currentMax < targetMax - 10) {
        return `Your maximum is ${currentMax}, but you need ${targetMax}. Move the highest point up.`;
      } else if (currentMax > targetMax + 10) {
        return `Your maximum is ${currentMax}, but you need ${targetMax}. Move the highest point down.`;
      }
      return `Almost there! Adjust the points to get exactly min=${targetMin} and max=${targetMax}.`;
    }

    return 'Try adjusting the points on the y-axis to achieve the target range.';
  };

  return (
    <div className={styles.tutor}>
      {/* ... существующий код ... */}
      
      {challenge && (
        <div className={`${styles['tutor-section']} ${styles['challenge-section']}`}>
          <h2>🎯 Your Challenge</h2>
          <p className={styles['challenge-text']}>{challenge.description}</p>
          <div className={styles['challenge-details']}>
            {/* ... существующий код отображения challenge ... */}
          </div>
          
          <button 
            onClick={() => setShowHint(!showHint)}
            style={{
              marginTop: '10px',
              padding: '8px 16px',
              background: '#667eea',
              color: 'white',
              border: 'none',
              borderRadius: '6px',
              cursor: 'pointer',
              fontSize: '14px'
            }}
          >
            {showHint ? 'Hide Hint' : '💡 Show Hint'}
          </button>
          
          {showHint && (
            <div style={{
              marginTop: '10px',
              padding: '12px',
              background: '#fff3cd',
              border: '1px solid #ffc107',
              borderRadius: '6px',
              fontSize: '14px'
            }}>
              {getHint()}
            </div>
          )}
        </div>
      )}
    </div>
  );
}

export default Tutor;
```

#### Шаг 2: Обновить `frontend/src/App.jsx`
Передать `points` в компонент Tutor:

```jsx
<Tutor challenge={challenge} points={points} />
```

### Время выполнения: 20-25 минут

---

## 9. Добавить отображение координат под графиком (Вероятность: 52%)

### Описание
Добавить таблицу или список, показывающий координаты всех точек под графиком для удобства пользователя.

### Решение

#### Обновить `frontend/src/components/BubbleGraph.jsx`
Добавить отображение координат в конце компонента:

```jsx
// В конце компонента, перед закрывающим div:
<div className={styles['coordinates-table']}>
  <h4 style={{ marginBottom: '10px', fontSize: '16px', fontWeight: 600 }}>
    Point Coordinates
  </h4>
  <table style={{ width: '100%', borderCollapse: 'collapse' }}>
    <thead>
      <tr style={{ background: '#f8f9fa' }}>
        <th style={{ padding: '8px', textAlign: 'left', border: '1px solid #ddd' }}>Name</th>
        <th style={{ padding: '8px', textAlign: 'center', border: '1px solid #ddd' }}>
          {graphData.xAxis.label}
        </th>
        <th style={{ padding: '8px', textAlign: 'center', border: '1px solid #ddd' }}>
          {graphData.yAxis.label}
        </th>
      </tr>
    </thead>
    <tbody>
      {points.map((point) => (
        <tr key={point.id}>
          <td style={{ padding: '8px', border: '1px solid #ddd' }}>{point.name}</td>
          <td style={{ padding: '8px', textAlign: 'center', border: '1px solid #ddd' }}>
            {point.x}
          </td>
          <td style={{ padding: '8px', textAlign: 'center', border: '1px solid #ddd' }}>
            {point.y}
          </td>
        </tr>
      ))}
    </tbody>
  </table>
</div>
```

Или более простой вариант со списком:

```jsx
<div className={styles['coordinates-list']} style={{
  marginTop: '20px',
  padding: '15px',
  background: '#f8f9fa',
  borderRadius: '8px'
}}>
  <h4 style={{ marginBottom: '10px', fontSize: '16px', fontWeight: 600 }}>
    Point Coordinates
  </h4>
  <div style={{ display: 'flex', flexWrap: 'wrap', gap: '10px' }}>
    {points.map((point) => (
      <div key={point.id} style={{
        padding: '8px 12px',
        background: 'white',
        borderRadius: '6px',
        border: '1px solid #ddd',
        fontSize: '14px'
      }}>
        <strong>{point.name}:</strong> ({point.x}, {point.y})
      </div>
    ))}
  </div>
</div>
```

### Время выполнения: 10-15 минут

---

## 10. Добавить Undo/Redo функциональность (Вероятность: 50%)

### Описание
Добавить возможность отменять и возвращать изменения в позициях точек.

### Решение

#### Обновить `frontend/src/App.jsx`

```jsx
const [history, setHistory] = useState([]);
const [historyIndex, setHistoryIndex] = useState(-1);

const handleUndo = () => {
    if (historyIndex > 0) {
        const newIndex = historyIndex - 1;
        setHistoryIndex(newIndex);
        setPoints(history[newIndex]);
    }
};

const handleRedo = () => {
    if (historyIndex < history.length - 1) {
        const newIndex = historyIndex + 1;
        setHistoryIndex(newIndex);
        setPoints(history[newIndex]);
    }
};

const loadInitialData = async () => {
    try {
      const response = await axios.get(`${API_BASE_URL}/data/`);
      setGraphData(response.data);
      const initial = response.data.points;
      setInitialPoints(initial);
      setPoints(initial);
      setHistory([initial.map(p => ({ ...p }))]);
      setHistoryIndex(0);
      setLoading(false);
    } catch (error) {
      console.error('Error loading data:', error);
      setLoading(false);
    }
};

const handlePointUpdate = (updatedPoints) => {
    setPoints(updatedPoints);

    const newHistory = history.slice(0, historyIndex + 1);
    const snapshot = updatedPoints.map(p => ({ ...p }));

    newHistory.push(snapshot);
    setHistory(newHistory);
    setHistoryIndex(newHistory.length - 1);
};

// В JSX, добавить кнопки в controls:
<div className="controls">
  <button 
    className="btn btn-secondary" 
    onClick={handleUndo}
    disabled={historyIndex <= 0}
  >
    ↶ Undo
  </button>
  <button 
    className="btn btn-secondary" 
    onClick={handleRedo}
    disabled={historyIndex >= history.length - 1}
  >
    ↷ Redo
  </button>
  <button className="btn btn-secondary" onClick={handleReset}>
    Reset
  </button>
  <button className="btn btn-primary" onClick={handleSubmit}>
    Submit Answer
  </button>
</div>
```
---

## 11. Добавить анимацию при успехе (Вероятность: 45%)

### Описание
Добавить визуальную анимацию (например, конфетти или пульсацию) при правильном ответе.

### Решение

#### Шаг 1: Обновить `frontend/src/components/Feedback.jsx`

```jsx
import React, { useEffect, useState } from 'react';
import styles from './Feedback.module.css';

function Feedback({ feedback, onNewChallenge }) {
  const [showAnimation, setShowAnimation] = useState(false);

  useEffect(() => {
    if (feedback?.is_correct) {
      setShowAnimation(true);
      const timer = setTimeout(() => setShowAnimation(false), 2000);
      return () => clearTimeout(timer);
    }
  }, [feedback]);

  if (!feedback) return null;

  return (
    <div className={`${styles['feedback-section']} ${feedback.is_correct ? styles.correct : styles.incorrect}`}>
      {showAnimation && (
        <div className={styles['celebration']}>
          <div className={styles['confetti']}>🎉</div>
          <div className={styles['confetti']}>✨</div>
          <div className={styles['confetti']}>🎊</div>
          <div className={styles['confetti']}>⭐</div>
        </div>
      )}
      <h2 className={feedback.is_correct && showAnimation ? styles['pulse'] : ''}>
        {feedback.is_correct ? '✅ Correct!' : '❌ Try Again'}
      </h2>
      {/* ... остальной код ... */}
    </div>
  );
}

export default Feedback;
```

#### Шаг 2: Обновить `frontend/src/components/Feedback.module.css`

```css
/* Добавить в конец файла: */

.celebration {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  pointer-events: none;
  overflow: hidden;
}

.confetti {
  position: absolute;
  font-size: 30px;
  animation: confetti-fall 2s ease-out forwards;
}

.confetti:nth-child(1) {
  left: 20%;
  animation-delay: 0s;
}

.confetti:nth-child(2) {
  left: 40%;
  animation-delay: 0.2s;
}

.confetti:nth-child(3) {
  left: 60%;
  animation-delay: 0.4s;
}

.confetti:nth-child(4) {
  left: 80%;
  animation-delay: 0.6s;
}

@keyframes confetti-fall {
  0% {
    transform: translateY(-100px) rotate(0deg);
    opacity: 1;
  }
  100% {
    transform: translateY(500px) rotate(720deg);
    opacity: 0;
  }
}

.pulse {
  animation: pulse 0.5s ease-in-out 3;
}

@keyframes pulse {
  0%, 100% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.1);
  }
}

.feedback-section {
  position: relative;
  /* ... остальные стили ... */
}
```

### Время выполнения: 15-20 минут

---

## 12. Добавить точку кликом по графику (Вероятность: 45%)

### Описание
Добавить возможность создавать новую точку на графике, кликая на пустую область графика.

### Решение

#### Обновить `frontend/src/components/BubbleGraph.jsx`

Добавить обработчик клика на график:

```jsx
const handleChartClick = useCallback((e) => {
    if (e.target.tagName === 'circle') {
        return;
    }
    
    const bounds = getChartBounds();
    if (!bounds) return;
    
    const rect = e.currentTarget.getBoundingClientRect();
    const clickX = e.clientX - rect.left;
    const clickY = e.clientY - rect.top;
    
    const marginLeft = 60;
    const marginRight = 20;
    const marginTop = 20;
    const marginBottom = 60;
    
    if (clickX < marginLeft || clickX > rect.width - marginRight ||
        clickY < marginTop || clickY > rect.height - marginBottom) {
        return;
    }
    
    const chartWidth = rect.width - marginLeft - marginRight;
    const chartHeight = rect.height - marginTop - marginBottom;
    
    const x = graphData.xAxis.min + 
        ((clickX - marginLeft) / chartWidth) * 
        (graphData.xAxis.max - graphData.xAxis.min);
    
    const y = graphData.yAxis.max - 
        ((clickY - marginTop) / chartHeight) * 
        (graphData.yAxis.max - graphData.yAxis.min);
    
    const clampedX = Math.max(
        graphData.xAxis.min,
        Math.min(graphData.xAxis.max, x)
    );
    const clampedY = Math.max(
        graphData.yAxis.min,
        Math.min(graphData.yAxis.max, y)
    );
    
    const defaultSize =
        graphData?.bubbleSize?.values?.[1] ??
        graphData?.bubbleSize?.values?.[0] ??
        2.5;

    const newPoint = {
        id: `point-${Date.now()}-${Math.random().toString(36).substr(2, 9)}`,
        name: `Point ${points.length + 1}`,
        x: Math.round(clampedX),
        y: Math.round(clampedY),
        size: defaultSize
    };
    
    const updatedPoints = [...points, newPoint];
    onPointUpdate(updatedPoints);
}, [getChartBounds, graphData, points, onPointUpdate]);

// В JSX, добавить onClick на контейнер графика:
<div
  ref={containerRef}
  className={styles['chart-wrapper']}
  style={{ cursor: 'crosshair', position: 'relative' }}
  onClick={handleChartClick}
>
```

#### Обновить `frontend/src/App.jsx`

Убедиться, что `handlePointUpdate` корректно обрабатывает добавление точек:

```jsx
const handlePointUpdate = (updatedPoints) => {
  setPoints(updatedPoints);
};
```

### Время выполнения: 20-25 минут

---

## 13. Экспорт статистики в JSON (кнопка → файл) (Вероятность: 42%)

### Описание
Добавить кнопку для экспорта статистики в JSON файл, который пользователь может скачать.

### Решение

#### Обновить `frontend/src/App.jsx`

```jsx
// Добавить функцию экспорта:
const handleExportStats = () => {
  const dataToExport = {
    statistics: statistics,
    exportDate: new Date().toISOString(),
    challengeHistory: [] // Можно добавить историю челленджей, если нужно
  };
  
  const jsonString = JSON.stringify(dataToExport, null, 2);
  const blob = new Blob([jsonString], { type: 'application/json' });
  const url = URL.createObjectURL(blob);
  
  const link = document.createElement('a');
  link.href = url;
  link.download = `range-app-stats-${new Date().toISOString().split('T')[0]}.json`;
  document.body.appendChild(link);
  link.click();
  document.body.removeChild(link);
  URL.revokeObjectURL(url);
};

// В JSX, добавить кнопку экспорта в statistics-dashboard:
<div className="statistics-dashboard">
  <div className="stat-item">
    <span className="stat-label">Completed:</span>
    <span className="stat-value">{statistics.completed}</span>
  </div>
  <div className="stat-item">
    <span className="stat-label">Total:</span>
    <span className="stat-value">{statistics.total}</span>
  </div>
  <div className="stat-item">
    <span className="stat-label">Success Rate:</span>
    <span className="stat-value">{statistics.successRate}%</span>
  </div>
  <button 
    className="btn-reset-stats" 
    onClick={handleResetStats} 
    title="Reset Statistics"
  >
    🔄
  </button>
  <button 
    className="btn-export-stats" 
    onClick={handleExportStats}
    title="Export Statistics"
    style={{
      marginLeft: '10px',
      padding: '8px 12px',
      background: '#667eea',
      color: 'white',
      border: 'none',
      borderRadius: '6px',
      cursor: 'pointer',
      fontSize: '14px'
    }}
  >
    📥 Export JSON
  </button>
</div>
```

### Время выполнения: 15-20 минут

---

## 14. Добавить удаление точек (Вероятность: 40%)

### Описание
Добавить возможность удалять точки с графика (например, двойным кликом или через контекстное меню).

### Решение

#### Обновить `frontend/src/components/BubbleGraph.jsx`

Добавить обработчик удаления точки:

```jsx
// Добавить функцию удаления точки
const handlePointDelete = useCallback((pointId, e) => {
  e.preventDefault();
  e.stopPropagation();
  
  // Удаляем точку из списка
  const updatedPoints = points.filter(p => p.id !== pointId);
  onPointUpdate(updatedPoints);
}, [points, onPointUpdate]);

// Обновить renderCustomShape для добавления обработчика удаления:
const renderCustomShape = useCallback((props) => {
  const { cx, cy, payload, z } = props;
  if (cx == null || cy == null) return null;
  
  const radius = z / 2;
  const pointId = payload.id;
  const isDragged = draggedPoint === pointId;
  
  return (
    <g>
      <circle
        cx={cx}
        cy={cy}
        r={radius}
        fill={isDragged ? '#764ba2' : '#667eea'}
        stroke="#fff"
        strokeWidth={2}
        style={{ cursor: 'pointer' }}
        onMouseDown={(e) => {
          e.preventDefault();
          e.stopPropagation();
          handleBubbleMouseDown(e, payload);
        }}
      />
      {/* Добавляем кнопку удаления (появляется при hover) */}
      <g
        className="delete-button"
        style={{
          opacity: 0,
          transition: 'opacity 0.2s',
          cursor: 'pointer'
        }}
        onMouseEnter={(e) => {
          e.currentTarget.style.opacity = 1;
        }}
        onMouseLeave={(e) => {
          e.currentTarget.style.opacity = 0;
        }}
        onDoubleClick={(e) => handlePointDelete(pointId, e)}
      >
        <circle
          cx={cx}
          cy={cy - radius - 15}
          r={8}
          fill="#ff6b6b"
          stroke="#fff"
          strokeWidth={2}
        />
        <text
          x={cx}
          y={cy - radius - 15}
          textAnchor="middle"
          dominantBaseline="middle"
          fill="#fff"
          fontSize="10"
          fontWeight="bold"
        >
          ×
        </text>
      </g>
    </g>
  );
}, [draggedPoint, handleBubbleMouseDown, handlePointDelete]);
```

Альтернативный вариант - удаление по двойному клику прямо на точку:

```jsx
const renderCustomShape = useCallback((props) => {
  const { cx, cy, payload, z } = props;
  if (cx == null || cy == null) return null;
  
  const radius = z / 2;
  const pointId = payload.id;
  const isDragged = draggedPoint === pointId;
  
  const point = points.find(p => p.id === pointId);
  if (!point) return null;
  
  let clickTimeout = null;
  
  return (
    <circle
      cx={cx}
      cy={cy}
      r={radius}
      fill={isDragged ? '#764ba2' : '#667eea'}
      stroke="#fff"
      strokeWidth={2}
      style={{ cursor: 'pointer' }}
      onMouseDown={(e) => {
        e.preventDefault();
        e.stopPropagation();
        
        // Проверяем двойной клик
        if (clickTimeout) {
          clearTimeout(clickTimeout);
          clickTimeout = null;
          // Это двойной клик - удаляем точку
          handlePointDelete(pointId, e);
        } else {
          // Одиночный клик - начинаем перетаскивание
          clickTimeout = setTimeout(() => {
            clickTimeout = null;
            handleBubbleMouseDown(e, point);
          }, 200);
        }
      }}
    />
  );
}, [draggedPoint, points, handleBubbleMouseDown, handlePointDelete]);
```

Более простой вариант - удаление через правый клик (контекстное меню):

```jsx
const handlePointContextMenu = useCallback((pointId, e) => {
  e.preventDefault();
  
  if (window.confirm('Delete this point?')) {
    const updatedPoints = points.filter(p => p.id !== pointId);
    onPointUpdate(updatedPoints);
  }
}, [points, onPointUpdate]);

// В renderCustomShape:
<circle
  // ... остальные атрибуты ...
  onContextMenu={(e) => handlePointContextMenu(pointId, e)}
/>
```

#### Обновить `frontend/src/App.jsx`

Убедиться, что минимальное количество точек проверяется (если нужно сохранить минимум):

```jsx
const handlePointUpdate = (updatedPoints) => {
  // Опционально: проверка минимального количества точек
  if (updatedPoints.length < 2) {
    alert('You need at least 2 points on the graph.');
    return;
  }
  setPoints(updatedPoints);
};
```

### Время выполнения: 20-25 минут

---

## 15. Добавить фильтрацию челленджей по типу (Вероятность: 40%)

### Описание
Добавить возможность выбирать тип челленджа на фронтенде и фильтровать на бэкенде.

### Решение

#### Шаг 1: Обновить `backend/api/views.py`

```python
@api_view(['GET'])
def get_challenge(request):
    """Generate a random challenge for the user."""
    try:
        challenge_type = request.query_params.get('type', None)
        
        if challenge_type:
            # Фильтровать по типу
            filtered_challenges = [c for c in CHALLENGE_TYPES if c['type'] == challenge_type]
            if filtered_challenges:
                challenge = random.choice(filtered_challenges)
            else:
                challenge = random.choice(CHALLENGE_TYPES)
        else:
            challenge = random.choice(CHALLENGE_TYPES)
        
        return Response(challenge)
    except Exception as e:
        return Response(
            {"error": str(e)},
            status=status.HTTP_500_INTERNAL_SERVER_ERROR
        )
```

#### Шаг 2: Обновить `frontend/src/App.jsx`

```jsx
const [selectedChallengeType, setSelectedChallengeType] = useState(null);

const loadChallenge = async (type = null) => {
  try {
    const url = type 
      ? `${API_BASE_URL}/challenge/?type=${type}`
      : `${API_BASE_URL}/challenge/`;
    const response = await axios.get(url);
    setChallenge(response.data);
    setFeedback(null);
  } catch (error) {
    console.error('Error loading challenge:', error);
  }
};

// В JSX, добавить перед Tutor:
<div style={{ marginBottom: '20px', textAlign: 'center' }}>
  <label style={{ marginRight: '10px', fontWeight: 600 }}>Challenge Type:</label>
  <select 
    value={selectedChallengeType || ''}
    onChange={(e) => {
      const type = e.target.value || null;
      setSelectedChallengeType(type);
      loadChallenge(type);
    }}
    style={{
      padding: '8px 12px',
      borderRadius: '6px',
      border: '1px solid #ddd',
      fontSize: '14px'
    }}
  >
    <option value="">Random</option>
    <option value="less_than">Less Than</option>
    <option value="greater_than">Greater Than</option>
    <option value="between">Between</option>
    <option value="exact">Exact</option>
  </select>
</div>
```

### Время выполнения: 20-25 минут

---

## 16. Добавить endpoint для истории попыток (Вероятность: 35%)

### Описание
Создать модель для сохранения попыток пользователя и endpoint для получения истории.

### Решение

#### Шаг 1: Создать модель в `backend/api/models.py`

```python
from django.db import models

class Attempt(models.Model):
    challenge_type = models.CharField(max_length=50)
    challenge_value = models.IntegerField(null=True, blank=True)
    challenge_min = models.IntegerField(null=True, blank=True)
    challenge_max = models.IntegerField(null=True, blank=True)
    user_range = models.IntegerField()
    is_correct = models.BooleanField()
    created_at = models.DateTimeField(auto_now_add=True)

    class Meta:
        ordering = ['-created_at']
```

#### Шаг 2: Создать миграцию

```bash
cd backend
python manage.py makemigrations
python manage.py migrate
```

#### Шаг 3: Обновить `backend/api/views.py`

```python
from .models import Attempt
from django.utils import timezone

@api_view(['POST'])
def validate_range(request):
    """Validate if the user's graph meets the challenge requirements."""
    # ... существующий код валидации ...
    
    # Сохранить попытку
    challenge_type = challenge.get('type')
    attempt = Attempt(
        challenge_type=challenge_type,
        challenge_value=challenge.get('value'),
        challenge_min=challenge.get('min'),
        challenge_max=challenge.get('max'),
        user_range=current_range,
        is_correct=is_correct
    )
    attempt.save()
    
    return Response({
        "is_correct": is_correct,
        "feedback": feedback,
        "current_range": {
            "min": current_min,
            "max": current_max,
            "range": current_range
        }
    })

@api_view(['GET'])
def get_attempt_history(request):
    """Get history of user attempts."""
    limit = int(request.query_params.get('limit', 10))
    attempts = Attempt.objects.all()[:limit]
    
    data = [{
        'id': a.id,
        'challenge_type': a.challenge_type,
        'challenge_value': a.challenge_value,
        'challenge_min': a.challenge_min,
        'challenge_max': a.challenge_max,
        'user_range': a.user_range,
        'is_correct': a.is_correct,
        'created_at': a.created_at.isoformat()
    } for a in attempts]
    
    return Response(data)
```

#### Шаг 4: Обновить `backend/api/urls.py`

```python
urlpatterns = [
    path('data/', views.get_initial_data, name='get_initial_data'),
    path('challenge/', views.get_challenge, name='get_challenge'),
    path('validate/', views.validate_range, name='validate_range'),
    path('history/', views.get_attempt_history, name='get_attempt_history'),
]
```

### Время выполнения: 25-30 минут

---

## 17. Ограничить перемещение точек только по X (Вероятность: 32%)

### Описание
Ограничить перемещение точек только по оси X (противоположность задаче #3, где ограничивают только Y).

### Решение

#### Обновить `frontend/src/components/BubbleGraph.jsx`
Изменить функцию `handleMouseMove`:

```jsx
const handleMouseMove = useCallback((e) => {
  if (!draggedPoint) return;
  
  const bounds = getChartBounds();
  if (!bounds) return;
  
  // Используем только deltaX - движение по X
  const deltaX = e.clientX - dragStart.x;
  
  // Вычисляем изменение по X
  const xDelta = (deltaX / bounds.width) * (graphData.xAxis.max - graphData.xAxis.min);
  
  // Оставляем Y без изменений
  const newY = dragStart.pointY; // Не изменяем Y
  
  const newX = Math.max(
    graphData.xAxis.min,
    Math.min(graphData.xAxis.max, dragStart.pointX + xDelta)
  );
  
  const updatedPoints = points.map((p) =>
    p.id === draggedPoint
      ? { ...p, x: Math.round(newX), y: Math.round(newY) }
      : p
  );
  
  onPointUpdate(updatedPoints);
}, [draggedPoint, dragStart, graphData, getChartBounds, points, onPointUpdate]);
```

### Время выполнения: 10-15 минут

---

## 18. Добавить звуковые эффекты (Вероятность: 30%)

### Описание
Добавить звуковые эффекты при успехе или неудаче.

### Решение

#### Шаг 1: Обновить `frontend/src/components/Feedback.jsx`

```jsx
import React, { useEffect, useRef } from 'react';
import styles from './Feedback.module.css';

function Feedback({ feedback, onNewChallenge }) {
  const audioRef = useRef(null);

  useEffect(() => {
    if (feedback?.is_correct) {
      // Создать звук успеха (beep)
      playSuccessSound();
    } else if (feedback && !feedback.is_correct) {
      // Создать звук неудачи
      playFailureSound();
    }
  }, [feedback]);

  const playSuccessSound = () => {
    const audioContext = new (window.AudioContext || window.webkitAudioContext)();
    const oscillator = audioContext.createOscillator();
    const gainNode = audioContext.createGain();

    oscillator.connect(gainNode);
    gainNode.connect(audioContext.destination);

    oscillator.frequency.value = 800;
    oscillator.type = 'sine';

    gainNode.gain.setValueAtTime(0.3, audioContext.currentTime);
    gainNode.gain.exponentialRampToValueAtTime(0.01, audioContext.currentTime + 0.5);

    oscillator.start(audioContext.currentTime);
    oscillator.stop(audioContext.currentTime + 0.5);
  };

  const playFailureSound = () => {
    const audioContext = new (window.AudioContext || window.webkitAudioContext)();
    const oscillator = audioContext.createOscillator();
    const gainNode = audioContext.createGain();

    oscillator.connect(gainNode);
    gainNode.connect(audioContext.destination);

    oscillator.frequency.value = 200;
    oscillator.type = 'sawtooth';

    gainNode.gain.setValueAtTime(0.3, audioContext.currentTime);
    gainNode.gain.exponentialRampToValueAtTime(0.01, audioContext.currentTime + 0.3);

    oscillator.start(audioContext.currentTime);
    oscillator.stop(audioContext.currentTime + 0.3);
  };

  if (!feedback) return null;

  return (
    <div className={`${styles['feedback-section']} ${feedback.is_correct ? styles.correct : styles.incorrect}`}>
      {/* ... остальной код ... */}
    </div>
  );
}

export default Feedback;
```

**Альтернативный вариант** (если нужны реальные звуковые файлы):

1. Добавить звуковые файлы в `frontend/public/sounds/`
2. Использовать HTML5 Audio:

```jsx
const playSuccessSound = () => {
  const audio = new Audio('/sounds/success.mp3');
  audio.play().catch(e => console.log('Audio play failed:', e));
};

const playFailureSound = () => {
  const audio = new Audio('/sounds/failure.mp3');
  audio.play().catch(e => console.log('Audio play failed:', e));
};
```

### Время выполнения: 15-20 минут
    // Если клик был по точке, не создаём новую
    return;
  }
  
  const bounds = getChartBounds();
  if (!bounds) return;
  
  // Получаем координаты клика относительно контейнера графика
  const rect = e.currentTarget.getBoundingClientRect();
  const clickX = e.clientX - rect.left;
  const clickY = e.clientY - rect.top;
  
  // Проверяем, что клик был в области графика (не на осях)
  const marginLeft = 60; // примерное значение margin для осей
  const marginRight = 20;
  const marginTop = 20;
  const marginBottom = 60;
  
  if (clickX < marginLeft || clickX > rect.width - marginRight ||
      clickY < marginTop || clickY > rect.height - marginBottom) {
    return; // Клик вне области графика
  }
  
  // Преобразуем координаты клика в координаты данных
  const chartWidth = rect.width - marginLeft - marginRight;
  const chartHeight = rect.height - marginTop - marginBottom;
  
  const x = graphData.xAxis.min + 
    ((clickX - marginLeft) / chartWidth) * 
    (graphData.xAxis.max - graphData.xAxis.min);
  
  const y = graphData.yAxis.max - 
    ((clickY - marginTop) / chartHeight) * 
    (graphData.yAxis.max - graphData.yAxis.min);
  
  // Ограничиваем значения по осям
  const clampedX = Math.max(
    graphData.xAxis.min,
    Math.min(graphData.xAxis.max, x)
  );
  const clampedY = Math.max(
    graphData.yAxis.min,
    Math.min(graphData.yAxis.max, y)
  );
  
  // Создаём новую точку
  const newPoint = {
    id: `point-${Date.now()}-${Math.random().toString(36).substr(2, 9)}`,
    name: `Point ${points.length + 1}`,
    x: Math.round(clampedX),
    y: Math.round(clampedY),
    z: 50 // размер по умолчанию
  };
  
  // Обновляем список точек
  const updatedPoints = [...points, newPoint];
  onPointUpdate(updatedPoints);
}, [getChartBounds, graphData, points, onPointUpdate]);

// В JSX, добавить onClick на контейнер графика:
<div 
  onClick={handleChartClick}
  style={{ cursor: 'crosshair', position: 'relative' }}
>
  {/* Компонент графика */}
</div>
```

Альтернативный вариант с использованием координат от Recharts:

```jsx
// Использовать onMouseDown на компоненте Scatter
<ScatterChart
  onMouseDown={(e) => {
    if (e.target.tagName === 'svg' || e.target.tagName === 'g') {
      handleChartClick(e);
    }
  }}
>
  {/* ... */}
</ScatterChart>
```

#### Обновить `frontend/src/App.jsx`

Убедиться, что `handlePointUpdate` корректно обрабатывает добавление точек:

```jsx
const handlePointUpdate = (updatedPoints) => {
  setPoints(updatedPoints);
};
```

### Время выполнения: 20-25 минут

---

## 18. Добавить удаление точек (Вероятность: 40%)

### Описание
Добавить возможность удалять точки с графика (например, двойным кликом или через контекстное меню).

### Решение

#### Обновить `frontend/src/components/BubbleGraph.jsx`

Добавить обработчик удаления точки:

```jsx
// Добавить функцию удаления точки
const handlePointDelete = useCallback((pointId, e) => {
  e.preventDefault();
  e.stopPropagation();
  
  // Удаляем точку из списка
  const updatedPoints = points.filter(p => p.id !== pointId);
  onPointUpdate(updatedPoints);
}, [points, onPointUpdate]);

// Обновить renderCustomShape для добавления обработчика удаления:
const renderCustomShape = useCallback((props) => {
  const { cx, cy, payload, z } = props;
  if (cx == null || cy == null) return null;
  
  const radius = z / 2;
  const pointId = payload.id;
  const isDragged = draggedPoint === pointId;
  
  return (
    <g>
      <circle
        cx={cx}
        cy={cy}
        r={radius}
        fill={isDragged ? '#764ba2' : '#667eea'}
        stroke="#fff"
        strokeWidth={2}
        style={{ cursor: 'pointer' }}
        onMouseDown={(e) => {
          e.preventDefault();
          e.stopPropagation();
          handleBubbleMouseDown(e, payload);
        }}
      />
      {/* Добавляем кнопку удаления (появляется при hover) */}
      <g
        className="delete-button"
        style={{
          opacity: 0,
          transition: 'opacity 0.2s',
          cursor: 'pointer'
        }}
        onMouseEnter={(e) => {
          e.currentTarget.style.opacity = 1;
        }}
        onMouseLeave={(e) => {
          e.currentTarget.style.opacity = 0;
        }}
        onDoubleClick={(e) => handlePointDelete(pointId, e)}
      >
        <circle
          cx={cx}
          cy={cy - radius - 15}
          r={8}
          fill="#ff6b6b"
          stroke="#fff"
          strokeWidth={2}
        />
        <text
          x={cx}
          y={cy - radius - 15}
          textAnchor="middle"
          dominantBaseline="middle"
          fill="#fff"
          fontSize="10"
          fontWeight="bold"
        >
          ×
        </text>
      </g>
    </g>
  );
}, [draggedPoint, handleBubbleMouseDown, handlePointDelete]);
```

Альтернативный вариант - удаление по двойному клику прямо на точку:

```jsx
const renderCustomShape = useCallback((props) => {
  const { cx, cy, payload, z } = props;
  if (cx == null || cy == null) return null;
  
  const radius = z / 2;
  const pointId = payload.id;
  const isDragged = draggedPoint === pointId;
  
  const point = points.find(p => p.id === pointId);
  if (!point) return null;
  
  let clickTimeout = null;
  
  return (
    <circle
      cx={cx}
      cy={cy}
      r={radius}
      fill={isDragged ? '#764ba2' : '#667eea'}
      stroke="#fff"
      strokeWidth={2}
      style={{ cursor: 'pointer' }}
      onMouseDown={(e) => {
        e.preventDefault();
        e.stopPropagation();
        
        // Проверяем двойной клик
        if (clickTimeout) {
          clearTimeout(clickTimeout);
          clickTimeout = null;
          // Это двойной клик - удаляем точку
          handlePointDelete(pointId, e);
        } else {
          // Одиночный клик - начинаем перетаскивание
          clickTimeout = setTimeout(() => {
            clickTimeout = null;
            handleBubbleMouseDown(e, point);
          }, 200);
        }
      }}
    />
  );
}, [draggedPoint, points, handleBubbleMouseDown, handlePointDelete]);
```

Более простой вариант - удаление через правый клик (контекстное меню):

```jsx
const handlePointContextMenu = useCallback((pointId, e) => {
  e.preventDefault();
  
  if (window.confirm('Delete this point?')) {
    const updatedPoints = points.filter(p => p.id !== pointId);
    onPointUpdate(updatedPoints);
  }
}, [points, onPointUpdate]);

// В renderCustomShape:
<circle
  // ... остальные атрибуты ...
  onContextMenu={(e) => handlePointContextMenu(pointId, e)}
/>
```

#### Обновить `frontend/src/App.jsx`

Убедиться, что минимальное количество точек проверяется (если нужно сохранить минимум):

```jsx
const handlePointUpdate = (updatedPoints) => {
  // Опционально: проверка минимального количества точек
  if (updatedPoints.length < 2) {
    alert('You need at least 2 points on the graph.');
    return;
  }
  setPoints(updatedPoints);
};
```

### Время выполнения: 20-25 минут

---

## 19. Добавить баг: при клике снова на сабмит при любом фидбеке в статистику в тотал добавляется 1 (Вероятность: 50%)

### Описание
Добавить баг, при котором при повторном клике на Submit (даже если feedback уже есть) в статистику в total добавляется 1.

### Решение

#### Обновить `frontend/src/App.jsx`

Проблема в том, что при каждом вызове `handleSubmit` статистика обновляется без проверки, был ли уже получен feedback для этого челленджа. Нужно убрать проверку или добавить баг:

```jsx
const handleSubmit = async () => {
  try {
    const response = await axios.post(`${API_BASE_URL}/validate/`, {
      points: points,
      challenge: challenge,
    });
    setFeedback(response.data);

    // БАГ: Статистика обновляется при каждом клике на Submit, 
    // даже если feedback уже есть. Это означает, что при повторном 
    // клике на Submit при любом фидбеке в total добавляется 1
    const newStats = {
      ...statistics,
      total: statistics.total + 1,  // Всегда добавляется 1, даже при повторном клике
      completed: response.data.is_correct ? statistics.completed + 1 : statistics.completed,
    };
    newStats.successRate = newStats.total > 0
      ? Math.round((newStats.completed / newStats.total) * 100)
      : 0;
    setStatistics(newStats);
    localStorage.setItem('rangeAppStatistics', JSON.stringify(newStats));
  } catch (error) {
    console.error('Error validating:', error);
    setFeedback({
      is_correct: false,
      feedback: 'Error validating your answer. Please try again.',
    });
  }
};
```

**Правильное решение (исправление бага):**
Добавить проверку, чтобы статистика обновлялась только один раз для каждого челленджа:

```jsx
const [lastSubmittedChallengeId, setLastSubmittedChallengeId] = useState(null);

const handleSubmit = async () => {
  try {
    const response = await axios.post(`${API_BASE_URL}/validate/`, {
      points: points,
      challenge: challenge,
    });
    setFeedback(response.data);

    // Исправление: проверяем, был ли уже отправлен этот челлендж
    const challengeId = challenge?.type + challenge?.value + challenge?.min + challenge?.max;
    if (lastSubmittedChallengeId !== challengeId) {
      const newStats = {
        ...statistics,
        total: statistics.total + 1,
        completed: response.data.is_correct ? statistics.completed + 1 : statistics.completed,
      };
      newStats.successRate = newStats.total > 0
        ? Math.round((newStats.completed / newStats.total) * 100)
        : 0;
      setStatistics(newStats);
      localStorage.setItem('rangeAppStatistics', JSON.stringify(newStats));
      setLastSubmittedChallengeId(challengeId);
    }
  } catch (error) {
    console.error('Error validating:', error);
    setFeedback({
      is_correct: false,
      feedback: 'Error validating your answer. Please try again.',
    });
  }
};
```

### Время выполнения: 10-15 минут

---

## 20. Авторизация простая через JWT (Вероятность: 60%)

### Описание
Добавить простую авторизацию через JWT токены. Пользователь должен иметь возможность залогиниться и получать токен, который затем используется для доступа к защищенным эндпоинтам.

### Решение

#### Шаг 1: Обновить `backend/requirements.txt`

```txt
PyJWT==2.8.0
```

#### Шаг 2: Обновить `backend/api/views.py`

```python
import jwt
from datetime import datetime, timedelta
from django.conf import settings
from rest_framework.decorators import api_view, permission_classes
from rest_framework.permissions import AllowAny

# JWT Secret Key (в продакшене должен быть в .env)
JWT_SECRET_KEY = getattr(settings, 'JWT_SECRET_KEY', 'your-secret-key-change-in-production')
JWT_ALGORITHM = 'HS256'


def generate_jwt_token(username):
    """Generate JWT token for user."""
    payload = {
        'username': username,
        'exp': datetime.utcnow() + timedelta(days=7),
        'iat': datetime.utcnow()
    }
    token = jwt.encode(payload, JWT_SECRET_KEY, algorithm=JWT_ALGORITHM)
    return token


def verify_jwt_token(token):
    """Verify JWT token and return username."""
    try:
        payload = jwt.decode(token, JWT_SECRET_KEY, algorithms=[JWT_ALGORITHM])
        return payload.get('username')
    except jwt.ExpiredSignatureError:
        return None
    except jwt.InvalidTokenError:
        return None


@api_view(['POST'])
@permission_classes([AllowAny])
def login(request):
    """Simple login endpoint that returns JWT token."""
    username = request.data.get('username')
    password = request.data.get('password')
    
    # Простая авторизация: любой username/password валидны
    # В реальном приложении здесь должна быть проверка в базе данных
    if username and password:
        token = generate_jwt_token(username)
        return Response({
            'token': token,
            'username': username
        })
    else:
        return Response(
            {'error': 'Username and password required'},
            status=status.HTTP_400_BAD_REQUEST
        )
```

#### Шаг 3: Обновить `backend/api/urls.py`

```python
urlpatterns = [
    path('data/', views.get_initial_data, name='get_initial_data'),
    path('challenge/', views.get_challenge, name='get_challenge'),
    path('validate/', views.validate_range, name='validate_range'),
    path('login/', views.login, name='login'),
]
```

#### Шаг 4: Обновить `frontend/src/App.jsx` для использования JWT

```jsx
const [token, setToken] = useState(localStorage.getItem('jwt_token'));

// Функция для логина
const handleLogin = async (username, password) => {
  try {
    const response = await axios.post(`${API_BASE_URL}/login/`, {
      username,
      password
    });
    const { token } = response.data;
    setToken(token);
    localStorage.setItem('jwt_token', token);
    // Устанавливаем токен в заголовки для всех последующих запросов
    axios.defaults.headers.common['Authorization'] = `Bearer ${token}`;
  } catch (error) {
    console.error('Login error:', error);
  }
};

// При загрузке приложения, если есть токен, устанавливаем его
useEffect(() => {
  if (token) {
    axios.defaults.headers.common['Authorization'] = `Bearer ${token}`;
  }
}, [token]);
```

### Время выполнения: 30-40 минут

---

## 21. Новый тип челленджа "average_y" (Вероятность: 70%)

### Описание
Добавить новый тип челленджа "average_y", который проверяет, чтобы среднее значение y-координат точек находилось в заданном диапазоне.

### Решение

#### Шаг 1: Обновить `backend/api/challenges.py`

Добавить новые челленджи в массив `CHALLENGE_TYPES`:

```python
{"type": "average_y", "min": 200, "max": 300, "description": "Make the average y-coordinate between 200 and 300"},
{"type": "average_y", "min": 250, "max": 350, "description": "Make the average y-coordinate between 250 and 350"},
{"type": "average_y", "min": 180, "max": 280, "description": "Make the average y-coordinate between 180 and 280"},
```

#### Шаг 2: Обновить `backend/api/views.py`

Добавить логику валидации в функцию `validate_range`:

```python
elif challenge_type == "average_y":
    min_val = challenge.get('min', 200)
    max_val = challenge.get('max', 300)
    average_y = sum(y_values) / len(y_values) if y_values else 0
    is_correct = min_val <= average_y <= max_val
    feedback = f"Your average y-coordinate is {average_y:.2f}. Target: between {min_val} and {max_val}. {'✓ Correct!' if is_correct else '✗ Try again!'}"
```

#### Шаг 3: Обновить `frontend/src/components/Tutor.jsx`

Добавить отображение нового типа челленджа:

```jsx
{challenge.type === 'average_y' && (
  <span>Target: Average y-coordinate between {challenge.min} and {challenge.max}</span>
)}
```

### Время выполнения: 15-20 минут

---

## 22. Новый тип челленджа "sum_in_range" (Вероятность: 70%)

### Описание
Добавить новый тип челленджа "sum_in_range", который проверяет, чтобы сумма всех y-координат попадала в заданный диапазон.

### Решение

#### Шаг 1: Обновить `backend/api/challenges.py`

Добавить новые челленджи в массив `CHALLENGE_TYPES`:

```python
{"type": "sum_in_range", "min": 1000, "max": 2000, "description": "Make the sum of all y-coordinates between 1000 and 2000"},
{"type": "sum_in_range", "min": 1500, "max": 2500, "description": "Make the sum of all y-coordinates between 1500 and 2500"},
{"type": "sum_in_range", "min": 800, "max": 1500, "description": "Make the sum of all y-coordinates between 800 and 1500"},
```

#### Шаг 2: Обновить `backend/api/views.py`

Добавить логику валидации в функцию `validate_range`:

```python
elif challenge_type == "sum_in_range":
    min_val = challenge.get('min', 1000)
    max_val = challenge.get('max', 2000)
    sum_y = sum(y_values)
    is_correct = min_val <= sum_y <= max_val
    feedback = f"Sum of all y-coordinates is {sum_y:.0f}. Target: between {min_val} and {max_val}. {'✓ Correct!' if is_correct else '✗ Try again!'}"
```

#### Шаг 3: Обновить `frontend/src/components/Tutor.jsx`

Добавить отображение нового типа челленджа:

```jsx
{challenge.type === 'sum_in_range' && (
  <span>Target: Sum of all y-coordinates between {challenge.min} and {challenge.max}</span>
)}
```

### Время выполнения: 15-20 минут

---

## 23. Добавить фильтры в /history (Вероятность: 45%)

### Описание
Добавить endpoint `/history` для получения истории попыток с поддержкой query-параметров для фильтрации:
- `?type=multiple_of` - фильтр по типу челленджа
- `?success=true` - фильтр по успешности (true/false)
- `?from=2024-01-01` - фильтр по дате (от указанной даты)

### Решение

#### Шаг 1: Создать модель в `backend/api/models.py`

```python
from django.db import models

class Attempt(models.Model):
    """Model to store user attempts for challenges."""
    challenge_type = models.CharField(max_length=50)
    challenge_value = models.IntegerField(null=True, blank=True)
    challenge_min = models.IntegerField(null=True, blank=True)
    challenge_max = models.IntegerField(null=True, blank=True)
    user_range = models.FloatField(null=True, blank=True)
    user_average_y = models.FloatField(null=True, blank=True)
    user_sum_y = models.FloatField(null=True, blank=True)
    is_correct = models.BooleanField()
    created_at = models.DateTimeField(auto_now_add=True)

    class Meta:
        ordering = ['-created_at']
```

#### Шаг 2: Создать миграцию

```bash
cd backend
python manage.py makemigrations
python manage.py migrate
```

#### Шаг 3: Обновить `backend/api/views.py`

Добавить сохранение попыток и endpoint для истории:

```python
from .models import Attempt
from datetime import datetime

# В функции validate_range, после валидации, добавить сохранение:
attempt = Attempt(
    challenge_type=challenge_type,
    challenge_value=challenge.get('value'),
    challenge_min=challenge.get('min'),
    challenge_max=challenge.get('max'),
    user_range=current_range,
    user_average_y=average_y if challenge_type == 'average_y' else None,
    user_sum_y=sum_y if challenge_type == 'sum_in_range' else None,
    is_correct=is_correct
)
attempt.save()

# Добавить новый endpoint:
@api_view(['GET'])
def get_attempt_history(request):
    """Get history of user attempts with filters."""
    # Фильтры из query-параметров
    challenge_type = request.query_params.get('type', None)
    success = request.query_params.get('success', None)
    from_date = request.query_params.get('from', None)
    
    # Получаем все попытки
    attempts = Attempt.objects.all()
    
    # Применяем фильтры
    if challenge_type:
        attempts = attempts.filter(challenge_type=challenge_type)
    
    if success is not None:
        success_bool = success.lower() == 'true'
        attempts = attempts.filter(is_correct=success_bool)
    
    if from_date:
        try:
            from_datetime = datetime.strptime(from_date, '%Y-%m-%d')
            attempts = attempts.filter(created_at__gte=from_datetime)
        except ValueError:
            pass  # Игнорируем неверный формат даты
    
    # Ограничиваем количество результатов
    limit = int(request.query_params.get('limit', 50))
    attempts = attempts[:limit]
    
    # Формируем ответ
    data = [{
        'id': a.id,
        'challenge_type': a.challenge_type,
        'challenge_value': a.challenge_value,
        'challenge_min': a.challenge_min,
        'challenge_max': a.challenge_max,
        'user_range': a.user_range,
        'user_average_y': a.user_average_y,
        'user_sum_y': a.user_sum_y,
        'is_correct': a.is_correct,
        'created_at': a.created_at.isoformat()
    } for a in attempts]
    
    return Response(data)
```

#### Шаг 4: Обновить `backend/api/urls.py`

```python
urlpatterns = [
    path('data/', views.get_initial_data, name='get_initial_data'),
    path('challenge/', views.get_challenge, name='get_challenge'),
    path('validate/', views.validate_range, name='validate_range'),
    path('history/', views.get_attempt_history, name='get_attempt_history'),
]
```

#### Шаг 5: Примеры использования

```javascript
// Получить все попытки
GET /api/history/

// Фильтр по типу
GET /api/history/?type=multiple_of

// Фильтр по успешности
GET /api/history/?success=true

// Фильтр по дате
GET /api/history/?from=2024-01-01

// Комбинация фильтров
GET /api/history/?type=less_than&success=true&from=2024-01-01

// С ограничением количества
GET /api/history/?limit=10
```

### Время выполнения: 25-30 минут

## 24. Переключатель темы светлая/темная

добавить кнопку/переключатель, который меняет цветовую схему приложения (темный/светлый фон). Сохранять выбор в localStorage.
  
  **Код решения:**

  **App.jsx:**
  ```jsx
  const [theme, setTheme] = useState(() => {
    const saved = localStorage.getItem('rangeAppTheme');
    return saved || 'light';
  });

  useEffect(() => {
    document.documentElement.setAttribute('data-theme', theme);
  }, [theme]);

  const toggleTheme = () => {
    const newTheme = theme === 'light' ? 'dark' : 'light';
    setTheme(newTheme);
    localStorage.setItem('rangeAppTheme', newTheme);
  };

  // В JSX добавить в header:
  <div className="header-row">
    <h1 className="title">Range Teaching App</h1>
    <button className="btn-theme" onClick={toggleTheme} title="Toggle theme">
      {theme === 'light' ? '🌙' : '☀️'}
    </button>
  </div>
  ```

  **App.css:**
  ```css
  :root[data-theme="light"] {
    --bg-primary: white;
    --bg-secondary: #f8f9fa;
    --text-primary: #333;
    --text-secondary: #666;
    --shadow: rgba(0, 0, 0, 0.3);
  }

  :root[data-theme="dark"] {
    --bg-primary: #1a1a2e;
    --bg-secondary: #16213e;
    --text-primary: #e4e4e4;
    --text-secondary: #b0b0b0;
    --shadow: rgba(0, 0, 0, 0.6);
  }

  .header-row {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .btn-theme {
    background: var(--bg-secondary);
    border: 2px solid var(--border-color);
    border-radius: 50%;
    width: 50px;
    height: 50px;
    font-size: 24px;
    cursor: pointer;
  }

  .container {
    background: var(--bg-primary);
    color: var(--text-primary);
  }
  ```

## 25. Среднее время решения
записывать время от начала челленджа до успешной валидации, рассчитывать и показывать среднее время.

- **Лучший результат по времени** — сохранять и отображать минимальное время, за которое был решен челлендж.
  
  **Код решения:**

  **App.jsx:**
  ```jsx
  const [challengeStartTime, setChallengeStartTime] = useState(null);
  
  const [statistics, setStatistics] = useState(() => {
    const saved = localStorage.getItem('rangeAppStatistics');
    return saved ? JSON.parse(saved) : { 
      completed: 0, 
      total: 0, 
      successRate: 0,
      averageTime: 0,
      bestTime: null,
      times: []
    };
  });

  const loadChallenge = async () => {
    try {
      const response = await axios.get(`${API_BASE_URL}/challenge/`);
      setChallenge(response.data);
      setFeedback(null);
      setChallengeStartTime(Date.now()); // Установить время начала
    } catch (error) {
      console.error('Error loading challenge:', error);
    }
  };

  const handleSubmit = async () => {
    try {
      const response = await axios.post(`${API_BASE_URL}/validate/`, {
        points: points,
        challenge: challenge,
      });
      setFeedback(response.data);

      let solveTime = null;
      if (challengeStartTime && response.data.is_correct) {
        solveTime = Math.round((Date.now() - challengeStartTime) / 1000);
      }

      const newStats = {
        ...statistics,
        total: statistics.total + 1,
        completed: response.data.is_correct ? statistics.completed + 1 : statistics.completed,
        times: response.data.is_correct 
          ? [...statistics.times, solveTime]
          : statistics.times,
      };
      
      newStats.successRate = newStats.total > 0
        ? Math.round((newStats.completed / newStats.total) * 100)
        : 0;
        
      newStats.averageTime = newStats.times.length > 0
        ? Math.round(newStats.times.reduce((a, b) => a + b, 0) / newStats.times.length)
        : 0;
        
      newStats.bestTime = newStats.times.length > 0
        ? Math.min(...newStats.times)
        : null;

      setStatistics(newStats);
      localStorage.setItem('rangeAppStatistics', JSON.stringify(newStats));
    } catch (error) {
      console.error('Error validating:', error);
      setFeedback({
        is_correct: false,
        feedback: 'Error validating your answer. Please try again.',
      });
    }
  };

  // В JSX добавить в statistics-dashboard:
  <div className="stat-item">
    <span className="stat-label">Avg Time:</span>
    <span className="stat-value">{statistics.averageTime}s</span>
  </div>
  <div className="stat-item">
    <span className="stat-label">Best Time:</span>
    <span className="stat-value">{statistics.bestTime !== null ? `${statistics.bestTime}s` : '-'}</span>
  </div>
  ```

## 26. Индикатор прогресса (процент)

рассчитывать процент выполнения (например, если цель range > 300, а текущий 150, то 50%). Показывать прогресс-бар.
  
  **Код решения:**

  **App.jsx - передать challenge в BubbleGraph:**
  ```jsx
  <BubbleGraph
    graphData={graphData}
    points={points}
    challenge={challenge}
    onPointUpdate={handlePointUpdate}
  />
  ```

  **BubbleGraph.jsx:**
  ```jsx
  function BubbleGraph({ graphData, points, challenge, onPointUpdate }) {
    // ... существующий код ...

    const calculateProgress = useMemo(() => {
      if (!challenge) return 0;
      
      const yValues = points.map(p => p.y);
      const min = Math.min(...yValues);
      const max = Math.max(...yValues);
      const currentRange = max - min;

      switch (challenge.type) {
        case 'greater_than':
          const target = challenge.value;
          return Math.min(100, (currentRange / target) * 100);
        
        case 'less_than':
          const maxPossible = graphData.yAxis.max - graphData.yAxis.min;
          return Math.min(100, ((target - currentRange) / maxPossible) * 100);
        
        case 'between':
          const rangeMin = challenge.min;
          const rangeMax = challenge.max;
          if (currentRange >= rangeMin && currentRange <= rangeMax) return 100;
          if (currentRange < rangeMin) {
            return Math.min(100, (currentRange / rangeMin) * 100);
          }
          return 0;
        
        case 'exact':
          // Для exact сложнее, можно упростить
          return 50; // placeholder
        
        default:
          return 0;
      }
    }, [points, challenge, graphData]);

    return (
      <div className={styles['bubble-graph-container']}>
        {/* ... существующий код ... */}
        
        <div className={styles['progress-container']}>
          <div className={styles['progress-label']}>
            Progress: {Math.round(calculateProgress)}%
          </div>
          <div className={styles['progress-bar']}>
            <div 
              className={styles['progress-fill']}
              style={{ width: `${calculateProgress}%` }}
            />
          </div>
        </div>
      </div>
    );
  }
  ```

  **BubbleGraph.module.css:**
  ```css
  .progress-container {
    margin-top: 20px;
    padding: 15px;
    background: #f8f9fa;
    border-radius: 8px;
  }

  .progress-label {
    text-align: center;
    margin-bottom: 10px;
    font-weight: 600;
    color: #333;
  }

  .progress-bar {
    width: 100%;
    height: 20px;
    background: #e0e0e0;
    border-radius: 10px;
    overflow: hidden;
  }

  .progress-fill {
    height: 100%;
    background: linear-gradient(90deg, #ff6b6b 0%, #ffd93d 50%, #51cf66 100%);
    transition: width 0.3s ease;
  }
  ```

## 27. Градиентная заливка между min и max

на графике закрасить область между минимальным и максимальным значениями градиентом (например, от красного к зеленому).
  
  **Код решения:**

  **BubbleGraph.jsx:**
  ```jsx
  import {
    // ... существующие импорты ...
    ReferenceArea,
  } from 'recharts';

  // В компоненте, в ScatterChart добавить:
  
  <ScatterChart
    margin={{ top: 20, right: 80, bottom: 60, left: 80 }}
  >
    <defs>
      <linearGradient id="rangeGradient" x1="0" y1="0" x2="0" y2="1">
        <stop offset="0%" stopColor="#ff6b6b" stopOpacity={0.3} />
        <stop offset="100%" stopColor="#51cf66" stopOpacity={0.3} />
      </linearGradient>
    </defs>
    
    <CartesianGrid strokeDasharray="3 3" />
    
    {/* ... XAxis, YAxis, ZAxis ... */}
    
    <ReferenceArea
      y1={currentRange.min}
      y2={currentRange.max}
      fill="url(#rangeGradient)"
      fillOpacity={0.4}
    />
    
    <ReferenceLine
      y={currentRange.min}
      stroke="#ff6b6b"
      strokeWidth={2}
      strokeDasharray="5 5"
      label={{ value: `Min: ${currentRange.min}`, position: "right" }}
    />
    <ReferenceLine
      y={currentRange.max}
      stroke="#51cf66"
      strokeWidth={2}
      strokeDasharray="5 5"
      label={{ value: `Max: ${currentRange.max}`, position: "right" }}
    />
    
    <Scatter
      name="Cities"
      data={scatterData}
      fill="#667eea"
      cursor="pointer"
      shape={renderCustomShape}
    />
  </ScatterChart>
  ```

## 28. Несколько наборов данных

добавить возможность переключения между разными наборами данных (не только Metro Systems, но и другие примеры). Новый endpoint `/api/datasets/` со списком доступных наборов.
  
  **Код решения:**

  **backend/api/graph_data.py:**
  ```python
  METRO_POINTS = [
      {"id": 1, "name": "Delhi", "x": 150, "y": 180, "size": 1.5},
      {"id": 2, "name": "Tokyo", "x": 190, "y": 200, "size": 2.5},
      # ... остальные точки ...
  ]

  METRO_CONFIG = {
      "title": "Metro Systems of the World",
      "xAxis": {
          "label": "Number of Stations",
          "min": 100,
          "max": 450,
          "step": 50
      },
      "yAxis": {
          "label": "Total System Length (km)",
          "min": 150,
          "max": 600,
          "step": 50
      },
      "bubbleSize": {
          "label": "Ridership (bn per year)",
          "values": [1.5, 2.5, 3.5]
      }
  }

  CITIES_POINTS = [
      {"id": 1, "name": "Paris", "x": 50, "y": 100, "size": 2.0},
      {"id": 2, "name": "London", "x": 60, "y": 150, "size": 2.5},
      # ... другие точки ...
  ]

  CITIES_CONFIG = {
      "title": "World Cities Population",
      "xAxis": {
          "label": "Area (km²)",
          "min": 0,
          "max": 200,
          "step": 20
      },
      "yAxis": {
          "label": "Population (millions)",
          "min": 0,
          "max": 300,
          "step": 30
      },
      "bubbleSize": {
          "label": "Density",
          "values": [1.0, 2.0, 3.0]
      }
  }

  DATASETS = {
      "metro": {
          "points": METRO_POINTS,
          "config": METRO_CONFIG
      },
      "cities": {
          "points": CITIES_POINTS,
          "config": CITIES_CONFIG
      }
  }

  # Для обратной совместимости
  GRAPH_POINTS = METRO_POINTS
  GRAPH_CONFIG = METRO_CONFIG
  ```

  **backend/api/views.py:**
  ```python
  from .graph_data import DATASETS

  @api_view(['GET'])
  def get_initial_data(request):
      """Return initial graph data based on dataset_id parameter."""
      dataset_id = request.query_params.get('dataset_id', 'metro')
      
      if dataset_id not in DATASETS:
          dataset_id = 'metro'  # fallback to default
      
      dataset = DATASETS[dataset_id]
      
      try:
          data = {
              **dataset["config"],
              "points": dataset["points"]
          }
          return Response(data)
      except Exception as e:
          return Response(
              {"error": str(e)},
              status=status.HTTP_500_INTERNAL_SERVER_ERROR
          )

  @api_view(['GET'])
  def get_datasets(request):
      """Return list of available datasets."""
      datasets_list = [
          {
              "id": key,
              "title": value["config"]["title"],
              "description": f"Dataset: {key}"
          }
          for key, value in DATASETS.items()
      ]
      return Response({"datasets": datasets_list})
  ```

  **backend/api/urls.py:**
  ```python
  urlpatterns = [
      path('data/', views.get_initial_data, name='get_initial_data'),
      path('challenge/', views.get_challenge, name='get_challenge'),
      path('validate/', views.validate_range, name='validate_range'),
      path('datasets/', views.get_datasets, name='get_datasets'),  # новый endpoint
  ]
  ```

  **frontend/src/App.jsx:**
  ```jsx
  const [datasets, setDatasets] = useState([]);
  const [selectedDataset, setSelectedDataset] = useState('metro');

  useEffect(() => {
    loadDatasets();
    loadInitialData();
    loadChallenge();
  }, []);

  useEffect(() => {
    if (selectedDataset) {
      loadInitialData();
    }
  }, [selectedDataset]);

  const loadDatasets = async () => {
    try {
      const response = await axios.get(`${API_BASE_URL}/datasets/`);
      setDatasets(response.data.datasets);
    } catch (error) {
      console.error('Error loading datasets:', error);
    }
  };

  const loadInitialData = async () => {
    try {
      const response = await axios.get(`${API_BASE_URL}/data/?dataset_id=${selectedDataset}`);
      setGraphData(response.data);
      const initial = response.data.points;
      setInitialPoints(initial);
      setPoints(initial);
      setLoading(false);
    } catch (error) {
      console.error('Error loading data:', error);
      setLoading(false);
    }
  };

  // В JSX добавить перед графиком:
  <div className="dataset-selector">
    <label>Select Dataset: </label>
    <select 
      value={selectedDataset} 
      onChange={(e) => setSelectedDataset(e.target.value)}
    >
      {datasets.map(ds => (
        <option key={ds.id} value={ds.id}>{ds.title}</option>
      ))}
    </select>
  </div>
  ```
