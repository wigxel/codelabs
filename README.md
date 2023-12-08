## DON'T REINVENT. UTILIZE

This repo can be seen as a Blog post. I'm documenting a code with friends section on the small yet powerful API's we use on a daily basis in Javascript Web development.

// iterate through use data
// rendering
// transforming
// Swagger Document ->
// Basic -> Swagger JSON -> Codegenator
// -> Transform SWAGGER -> fetcherFunctions & useQuery

const axios = {};
const GOOGLE_API_KEY = "GOOGLE_API_KEY";

export const getDistanceBetween = (cordA, cordB) => {
return axios.get(`maps/api/distancematrix/json`, {
params: new URLSearchParams({
origins: [cordA.lat, cordA.lng].join(','),
destinations: [cordB.lat, cordB.lng].join(','),
key: GOOGLE_API_KEY,
})
});
};

// getDistanceBetween({ lat: 12, lng: 205 }, { lat: 50, lng: 24 });
