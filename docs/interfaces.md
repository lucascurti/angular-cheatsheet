An interface is a specification identifying a related set of properties and methods. A class commits to supporting the specification by implementing the interface and then the interface can be used as a data type.

```ts
export interface ICar {
  carId: number;
  carName: string;
  carModel: string;
  remainingMiles(gas: number, speed:number): number;
}
```
```ts
import { ICar } from './car';

export class CarListComponent {
  cars: ICar[] = [...];
}
```
