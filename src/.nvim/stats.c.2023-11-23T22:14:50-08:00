#include <float.h>
#include <pthread.h>
#include <semaphore.h>
#include <stdio.h>
#include <stdlib.h>

int *factoryProductions;
int *eaten;
int factories;
double *minDelay;
double *maxDelay;
double *summedDelay;
pthread_mutex_t mutex_ = PTHREAD_MUTEX_INITIALIZER;

void stats_init(int num_producers) {
  factoryProductions = (int *)malloc(sizeof(int) * num_producers);
  eaten = (int *)malloc(sizeof(int) * num_producers);
  minDelay = (double *)malloc(sizeof(double) * num_producers);
  maxDelay = (double *)malloc(sizeof(double) * num_producers);
  summedDelay = (double *)malloc(sizeof(double) * num_producers);
  for (int i = 0; i < num_producers; i++) {
    factoryProductions[i] = 0;
    eaten[i] = 0;
    minDelay[i] = DBL_MAX;
    maxDelay[i] = 0;
    summedDelay[i] = 0;
  }
  factories = num_producers;
}

void stats_cleanup() {
  if (factoryProductions != NULL)
    free(factoryProductions);
  if (eaten != NULL)
    free(eaten);
  if (minDelay != NULL)
    free(minDelay);
  if (maxDelay != NULL)
    free(maxDelay);
  if (summedDelay != NULL)
    free(summedDelay);
}

void stats_record_produced(int factory_number) {
  pthread_mutex_lock(&mutex_);
  factoryProductions[factory_number] += 1;
  pthread_mutex_unlock(&mutex_);
}

void stats_record_consumed(int factory_number, double delay_in_ms) {
  pthread_mutex_lock(&mutex_);
  eaten[factory_number] += 1;
  double prevMin = minDelay[factory_number];
  double prevMax = maxDelay[factory_number];
  if (delay_in_ms <= prevMin)
    minDelay[factory_number] = delay_in_ms;
  if (delay_in_ms >= prevMax)
    maxDelay[factory_number] = delay_in_ms;
  summedDelay[factory_number] += delay_in_ms;
  pthread_mutex_unlock(&mutex_);
}

void stats_display() {
  printf("%8s%10s%10s%20s%20s%20s\n", "Factory#", "#Made", "#Eaten",
         "Min Delay[ms]", "Avg Delay[ms]", "Max Delay[ms]");
  for (int i = 0; i < factories; i++) {
    if (factoryProductions[i] == eaten[i]) {
      printf("%8d%10d%10d%20.5f%20.5f%20.5f\n", i, factoryProductions[i],
             eaten[i], minDelay[i], summedDelay[i] / eaten[i], maxDelay[i]);
    } else {
      printf("ERROR: Mismatch between number made and eaten.\n");
    }
  }
}
