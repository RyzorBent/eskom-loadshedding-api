# Eskom Loadshedding API

![Dependencies](https://img.shields.io/david/polarizedions/eskom-loadshedding-api?style=for-the-badge)
![Downloads](https://img.shields.io/npm/dm/eskom-loadshedding-api?style=for-the-badge&color=informational)
![Licence](https://img.shields.io/npm/l/eskom-loadshedding-api?style=for-the-badge&color=red)
![Version](https://img.shields.io/npm/v/eskom-loadshedding-api?style=for-the-badge)

A basic (in-progress) api to expose the Eskom loadshedding endpoints. Typescript ready!

## Usage / Examples

### Get the current status

```ts
import { Status, LoadsheddingStatus } from 'eskom-loadshedding-api';

Status.getStatus()
    .then((status: LoadsheddingStatus) =>
        console.log('Current status: ', status)
    );
```

### Check if currently loadshedding
```ts
import { Status, LoadsheddingStatus } from 'eskom-loadshedding-api';

Status.getStatus()
    .then(status =>
        console.log('Is currently loadshedding?', status !== LoadsheddingStatus.NOT_LOADSHEDDING)
    );
```

### Search for municipalities

```ts
import { Search, Province, Municipality } from 'eskom-loadshedding-api';

Search.getMunicipalities(Province.WESTERN_CAPE)
    .then((municipalities: Municipality[]) => 
        console.log('Western Cape municipalities:', municipalities.map((el: Municipality) => el.name))
    );

```

### Search for suburbs in municipalities

```ts
import { Search, Suburb } from 'eskom-loadshedding-api';

Search.getMunicipalitySuburbs(10237 /* Beauford West's id */, 'Aard' /* Search term */)
    .then((suburbs: Suburb[]) =>
        console.log('Filterd suburbs in Beaufort West:', suburbs)
    );
```

### Search for suburbs in SA

```ts
import { Search, SearchSuburb } from 'eskom-loadshedding-api';

Search.searchSuburbs('Ashton')
    .then((results: SearchSuburb[]) => 
        console.log('Searching for "Ashton":', results)
    );

```

### Get Schedule for suburb

```ts
import { LoadsheddingStage, LoadsheddingSchedule, Schedule } from 'eskom-loadshedding-api';

Schedule.getSchedule(1002702, LoadsheddingStage.STAGE_1)
    .then((schedule: LoadsheddingSchedule) =>
        console.log(JSON.stringify(schedule, null, 4))
);

Schedule.getFullSchedule(1002702)
    .then((schedules: LoadsheddingSchedule[]) =>
        console.log(JSON.stringify(schedules, null, 4))
);
```

## Methods

### Status
+ Status.getStatus(): Promise<LoadsheddingStatus>;
+ Status.getStatusRaw(): Promise<LoadsheddingStatusRaw>;

### Search
+ Search.getMunicipalities(province: Province): Promise<Municipality[]>;
+ Search.getMunicipalitySuburbs(municipalityId: number, searchTerm: string = '', pageNum: number = 1): Promise<Suburb[]>;
+ Search.searchSuburbs(searchTerm: string, maxResults: number = 300): Promise<SearchSuburb[]>;

### Schedule
+ Schedule.getSchedule(suburbId: number, stage: LoadsheddingStage): Promise<LoadsheddingSchedule>;
+ Schedule.getFullSchedule(suburbId: number): Promise<LoadsheddingSchedule[]>;

## Models

### Municipality

```ts
class Municipality {
    public id: number;
    public name: string;
}
```

### Suburb

```ts
class Suburb {
    public id: number;
    public name: string;
    public total: number;
}
```

### Search Suburb

```ts
class SearchSuburb {
    municipality: string;
    province: string;
    suburb: string;
    id: number;
    total: number;
}
```

### Schedule

```ts
export interface LoadsheddingSchedule {
    schedule: ScheduleDay[];
}

export interface ScheduleDay {
    day: Date;
    times: ScheduleTime[];
}

export interface ScheduleTime {
    startTime: Date;
    endTime: Date;
}

```

## Enums

### Loadshedding Stage
```ts
enum LoadsheddingStage {
    UNKNOWN = -1,
    NOT_LOADSHEDDING = 0,
    STAGE_1 = 1,
    STAGE_2 = 2,
    STAGE_3 = 3,
    STAGE_4 = 4,
    STAGE_5 = 5,
    STAGE_6 = 6,
    STAGE_7 = 7,
    STAGE_8 = 8,
}
```

### Province

```ts
enum Province {
    EASTERN_CAPE = 1,
    FREE_STATE = 2,
    GAUTENG = 3,
    KWAZULU_NATAL = 4,
    LIMPOPO = 5,
    MPUMALANGA = 6,
    NORTH_WEST = 7,
    NORTHERN_CAPE = 8,
    WESTERN_CAPE = 9,
};
```
