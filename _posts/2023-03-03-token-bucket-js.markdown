---
layout: post
title:  "Token Bucket algorithm: limiting api call rate in a javascript project"
date:   2023-03-03 16:00:00 +0600
img: post/token-bucket.png
categories: node, javascript, token-bucket
comments: true
---

In one JS project, we created an api for a client and needed to apply a rate limit. I preferred token-bucket approach. 
The rate limiter library is a single class with two published methods: `available`, `log`.
`log` method logs one use of the api, in database. The `available` method checks the availability of the api due to rate limiting.
Our limiting was hourly, so we created an array of fixed number of elements, eqaliting to the rate limit value. For each api use or `log`, one element from the array popped out.

The full implementation is below, note the simplicity of the code for such powerful application:

```
export const RateLimit = class {
    constructor(dbHelpers, apikey, allowed_rate_hr) {
        this.db = dbHelpers;
        this.apikey = apikey;
        this.allowed_rate_hr = allowed_rate_hr;
        this.use = {};
    }
    available() {
        const { current_hour_id } = this.db.current_time_info();
        const { last_use_hour_id, use_count } = this.db.api_use_info(this.apikey);
        const available_count = current_hour_id === last_use_hour_id? (this.allowed_rate_hr-use_count):(current_hour_id>last_use_hour_id?this.allowed_rate_hr:0);
        this.use[current_hour_id] = Array(available_count);
        return this.use[current_hour_id].length;
    }
    log() {
        const { current_hour_id } = this.db.current_time_info();
        this.use[current_hour_id].pop();
        this.db.save_api_use_info(this.apikey, current_hour_id, this.allowed_rate_hr - this.use[current_hour_id].length);
    }
};

export class RedisHelpers {
    constructor() {}
    current_time_info() {
        return {current_hour_id: Date.now()/(1000*3600)};
    }
    use_info(apikey) {
        return {
            last_use_hour_id: "TODO",
            use_count: "TODO"
        };
    }
    save_use_info(apikey, hour_id, use_count) {

    }
}
```
