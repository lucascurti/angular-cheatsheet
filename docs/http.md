<div class="filename">movie.service.ts</div>

```ts
import { Http, Response } from '@angular/http';
import { Observable } from 'rxjs/Observable';
import 'rxjs/add/operator/map';

@Injectable()
export class MovieService {
  private _movieUrl = 'www.myWebService.com/api/products';

  constructor(private _http: Http) { }

  getMovies(): Observable<IMovie[]> {
    return this._http.get(this._movieUrl)
      .map((response: Response) => <IMovie[]>response.json())
      .do(data => console.log(JSON.stringify(data)))
      .catch(this.handleError);
  }

  private handleError(error: Response) {
    console.error(error);
    return Observable.throw(error.json().error || 'Server error');
  }
}
```

<div class="filename">movies-list.component.ts</div>

```ts
import { MovieService } from './movie.service.ts';

@Component({
  selector: 'movies',
  templateUrl: 'movies-list.component.html'
})
export class MoviesComponent implements onInit {
  movies: IMovie[];
  errorMessage: string;

  constructor (private _movieService: MovieService) {
  }
  ngOnInit(): void {
    this._movieService.getMovies()
      .subscribe(movies => this.movies = movies,
                error => this.errorMessage = <any>error);
  }
}
```

Performs http requests using `XMLHttpRequest` as the default backend.

`Http` is available as an injectable class, with methods to perform http requests. Calling `request` returns an `Observable` which will emit a single `Response` when a response is received.

* `HttpModule` should be added to the `imports` array of one of the `@ngModule`