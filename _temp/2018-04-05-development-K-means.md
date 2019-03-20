---
layout: post
title: "DingDong의 K-means 응용"
categories:
  - Development
tags:
  - DingDong
  - K-means
---

<br>

### K-means 를 어떻게 응용했는지 알아보겠습니다.

<br>

우선 K-means란 K개의 그룹으로 데이터를 묶는 클러스터링 방법입니다.<br>
개념에 대한 자세한 정보는 [K-means 위키백과](https://ko.wikipedia.org/wiki/K-%ED%8F%89%EA%B7%A0_%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98 "K-means")를 참고하세요.

<br>

#### DingDong에서는 다음과 같이 응용하였습니다.

방문자가 초인종을 눌렀을때 지난 방문자의 기록을 DB에서 가져와
초인종이 자주 눌렸던 시간때를 K개 뽑아냅니다. 그리고 현재 시간과 비교해서
그 차이가 많아진다면 수상한 방문자가 되겠죠.??

<br>

#### 패턴 분석을 통한 방문자를 구분하여 다른 알림을 표시합니다.

<br>

1. 일반 방문자가 왔을때 알림
![new_notice](https://user-images.githubusercontent.com/31085727/38353690-2943a600-38f2-11e8-84d0-29f8eca8937d.png)

2. 수상한 방문자가 왔을때 알림
![suspicious_notice](https://user-images.githubusercontent.com/31085727/38353691-2972d2ae-38f2-11e8-9bfd-c7e6fabce909.png)

<br>

#### 구글링을 통한 코드 발췌.

JAVA로 작성된 K-means 코드는 구글링을 통해
[K-means 예제](https://picoledelimao.github.io/blog/2016/03/12/multithreaded-k-means-in-java/ "K-means 예제") 를 참고하였습니다.<br>
하지만 본 예제는 2차원, 즉 축이 2개로된 예제였습니다.<br>
제가 필요했던 것은 1차원 x축이 시간이 되는 코드였기 때문에 수정이 필요했습니다.

<br>

위 사이트의 코드에 각 클래스와 함수가 뜻하는 바를 주석으로 달았습니다.


```java
package kmeans.exam;

import java.io.BufferedReader;
import java.io.FileReader;
import java.util.ArrayList;
import java.util.List;
public class Kmeans {
	
	// (병렬화를 위해 )파일에서 읽을 점들복사 개수
	private static final int REPLICATION_FACTOR = 200;
	
	// 포인트 정적 내부 클래스
	public static class Point2D {
	
	    private float x;
	    private float y;
	
	    // 기준점 좌표 저장(생성자)
	    public Point2D(float x, float y) {
	        this.x = x;
	        this.y = y;
	    }
	    
	    // 기준점과 주어짐 점의 거리계산
	    private double getDistance(Point2D other) {
	        return Math.sqrt(Math.pow(this.x - other.x, 2)
	                + Math.pow(this.y - other.y, 2));
	    }
	    
	    // 기준점과 제일 가까운 점목록의 인덱스 반환
	    public int getNearestPointIndex(List<Point2D> points) {
	        int index = -1;
	        double minDist = Double.MAX_VALUE;
	        for (int i = 0; i < points.size(); i++) {
	            double dist = this.getDistance(points.get(i));
	            if (dist < minDist) {
	                minDist = dist;
	                index = i;
	            }
	        }
	        return index;
	    }
	    
	    // 점들의 평균(무게중심) 찾기
	    public static Point2D getMean(List<Point2D> points) {
	        float accumX = 0;
	        float accumY = 0;
	        if (points.size() == 0) return new Point2D(accumX, accumY);
	        for (Point2D point : points) {
	            accumX += point.x;
	            accumY += point.y;
	        }
	        return new Point2D(accumX / points.size(), accumY / points.size());
	    }
	    
	    
	    // 점의 좌표를 알려줄 toString
	    @Override
	    public String toString() {
	        return "[" + this.x + "," + this.y + "]";
	    }
	    
	    // 기준점과 주어진점이 같은지 판별
	    @Override
	    public boolean equals(Object obj) {
	        if (obj == null || !(obj.getClass() != Point2D.class)) {
	            return false;
	        }
	        Point2D other = (Point2D) obj;
	        return this.x == other.x && this.y == other.y;
	    } 
	}
	
	// 입력 파일에 대한 경로를 받고 점 목록 반환
	public static List<Point2D> getDataset(String inputFile) throws Exception {
	    List<Point2D> dataset = new ArrayList<>();
	    BufferedReader br = new BufferedReader(new FileReader(inputFile));
	    String line;
	    while ((line = br.readLine()) != null) {
	        String[] tokens = line.split(",");
	        float x = Float.valueOf(tokens[0]);
	        float y = Float.valueOf(tokens[1]);
	        Point2D point = new Point2D(x, y);
	        for (int i = 0; i < REPLICATION_FACTOR; i++)
	            dataset.add(point);
	    }
	    br.close();
	    return dataset;
	}
	
	// n개의 랜덤 중심점 목록을 반환
	// upperBound - 기존 집합의 원소보다 크거나 같은값
	// lowerBound - 기존 집합의 원소보다 작거나 같은값
	public static List<Point2D> initializeRandomCenters(int n, int lowerBound, int upperBound) {
	    List<Point2D> centers = new ArrayList<>(n);
	    for (int i = 0; i < n; i++) {
	        float x = (float)(Math.random() * (upperBound - lowerBound) + lowerBound);
	        float y = (float)(Math.random() * (upperBound - lowerBound) + lowerBound);
	        Point2D point = new Point2D(x, y);
	        centers.add(point);
	    }
	    return centers;
	}
	
	// 기존 중심점들을 데이터에 맞춰 새로운 중심점으로 반환
	public static List<Point2D> getNewCenters(List<Point2D> dataset, List<Point2D> centers) {
		// 클러스터 목록 생성(기존 중심점 크기만큼)
	    List<List<Point2D>> clusters = new ArrayList<>(centers.size());
	    for (int i = 0; i < centers.size(); i++) {
	        clusters.add(new ArrayList<Point2D>());
	    }
	    for (Point2D data : dataset) {
	    	// 더미의 각각의 점들과 가까운 중심점을 찾아 클러스터 목록에 추가
	        int index = data.getNearestPointIndex(centers);
	        clusters.get(index).add(data);
	    }
	    // 클러스터 목록에서 중심점을 찾음
	    List<Point2D> newCenters = new ArrayList<>(centers.size());
	    for (List<Point2D> cluster : clusters) {
	        newCenters.add(Point2D.getMean(cluster));
	    }
	    return newCenters;
	}
	
	// 기존 중심점들과 새로운 중심점들간 거리의 합 반환
	public static double getDistance(List<Point2D> oldCenters, List<Point2D> newCenters) {
		double accumDist = 0;
		for (int i = 0; i < oldCenters.size(); i++) {
			double dist = oldCenters.get(i).getDistance(newCenters.get(i));
			accumDist += dist;
		}
		return accumDist;
	}
	
	// 새로운 중심점이 고정될때까지 반복 수행
	public static List<Point2D> kmeans(List<Point2D> centers, List<Point2D> dataset, int k) {
	    boolean converged;
	    do {
	    	// 새로운 중심점 목록
	        List<Point2D> newCenters = getNewCenters(dataset, centers);
	        // 기존 중심점들과 새로운 중심점의 거리들간 거리의 합
	        double dist = getDistance(centers, newCenters);
	        centers = newCenters;
	        converged = dist == 0;
	    } while (!converged);
	    return centers;
	}
	
	// 메인 메서드	
	public static void main(String[] args) {
		// 프로그램은 입력파일 경로와 클러스터 수, 이 2개의 인수가 필요함
	    /*
		if (args.length != 2) {
	        System.err.println("Usage: KMeans <INPUT_FILE> <K>");
	        System.exit(-1);
	    }
	    String inputFile = args[0];
	    int k = Integer.valueOf(args[1]);
	    */
		
	    // 편의상 직접 넣어줬음
		String inputFile = "C://java97/workspace/kmeans/src/kmeans/data/points.txt";
		int k = 1;	    
	    
	    // 파일을 점목록(Ponit 2D 클래스를 담는 리스트)으로 바꿔줌
	    List<Point2D> dataset = null;
		try {
		    dataset = getDataset(inputFile);
		} catch (Exception e) {
		    System.err.println("ERROR: Could not read file " + inputFile);
		    System.exit(-1);
		}
	    
		List<Point2D> centers = initializeRandomCenters(k, 0, 1000000);
		long start = System.currentTimeMillis();
		// kmeans(centers, dataset, k);
		List<Point2D> kmeansCenterList = kmeans(centers, dataset, k); // <<<직접 넣어준 코드
		System.out.println("Time elapsed: " + (System.currentTimeMillis() - start) + "ms");

		for (Point2D center : kmeansCenterList) {
			System.out.println(center);
		}
		System.exit(0);
	}
	
}
```

<br>

y축 제거을 제거하고 거리 계산 코드 추가<br>

#### 수정된 코드입니다.

```java
package kmeans.exam;

import java.io.BufferedReader;
import java.io.FileReader;
import java.util.ArrayList;
import java.util.List;

// 단일축 k-means
public class Kmeans4 {
	
	// (병렬화를 위해 )파일에서 읽을 점들 복사 개수
	private static final int REPLICATION_FACTOR = 1;
	
	// 포인트 정적 내부 클래스
	public static class Point {
	
	    private float x;
	
	    // 기준점 좌표 저장(생성자)
	    public Point(float x) {
	        this.x = x;
	    }
	    
	    // 기준점과 주어짐 점의 거리계산
	    private double getDistance(Point other) {
	        return Math.sqrt(Math.pow(this.x - other.x, 2));
	    }
	    
	    // 기준점과 제일 가까운 점목록의 인덱스 반환
	    public int getNearestPointIndex(List<Point> points) {
	        int index = -1;
	        double minDist = Double.MAX_VALUE;
	        for (int i = 0; i < points.size(); i++) {
	            double dist = this.getDistance(points.get(i));
	            if (dist < minDist) {
	                minDist = dist;
	                index = i;
	            }
	        }
	        return index;
	    }
	    
	    // 점들의 평균(무게중심) 찾기
	    public static Point getMean(List<Point> points) {
	        float accumX = 0;
	        if (points.size() == 0) return new Point(accumX);
	        for (Point point : points) {
	            accumX += point.x;
	        }
	        return new Point(accumX / points.size());
	    }
	    
	    
	    // 점의 좌표를 알려줄 toString
	    @Override
	    public String toString() {
	        return "[" + this.x + "]";
	    }
	    
	    // 기준점과 주어진점이 같은지 판별
	    @Override
	    public boolean equals(Object obj) {
	        if (obj == null || !(obj.getClass() != Point.class)) {
	            return false;
	        }
	        Point other = (Point) obj;
	        return this.x == other.x;
	    } 
	}
	
	// 입력 파일에 대한 경로를 받고 점 목록 반환
	public static List<Point> getDataset(String inputFile) throws Exception {
	    List<Point> dataset = new ArrayList<>();
	    BufferedReader br = new BufferedReader(new FileReader(inputFile));
	    String line;
	    while ((line = br.readLine()) != null) {
	        String token = line;
	        float x = Float.valueOf(token);
	        Point point = new Point(x);
	        for (int i = 0; i < REPLICATION_FACTOR; i++)
	            dataset.add(point);
	    }
	    br.close();
	    // System.out.println(dataset);
	    return dataset;
	}
	
	// n개의 랜덤 중심점 목록을 반환
	// upperBound - 기존 집합의 원소보다 크거나 같은값
	// lowerBound - 기존 집합의 원소보다 작거나 같은값
	public static List<Point> initializeRandomCenters(int n, int lowerBound, int upperBound) {
	    List<Point> centers = new ArrayList<>(n);
	    for (int i = 0; i < n; i++) {
	        float x = (float)(Math.random() * (upperBound - lowerBound) + lowerBound);
	        Point point = new Point(x);
	        centers.add(point);
	    }
	    return centers;
	}
	
	// 기존 중심점들을 데이터에 맞춰 새로운 중심점으로 반환
	public static List<Point> getNewCenters(List<Point> dataset, List<Point> centers) {
		// 클러스터 목록 생성(기존 중심점 크기만큼)
	    List<List<Point>> clusters = new ArrayList<>(centers.size());
	    for (int i = 0; i < centers.size(); i++) {
	        clusters.add(new ArrayList<Point>());
	    }
	    for (Point data : dataset) {
	    	// 더미의 각각의 점들과 가까운 중심점을 찾아 클러스터 목록에 추가
	        int index = data.getNearestPointIndex(centers);
	        clusters.get(index).add(data);
	    }
	    // 클러스터 목록에서 중심점을 찾음
	    List<Point> newCenters = new ArrayList<>(centers.size());
	    for (List<Point> cluster : clusters) {
	        newCenters.add(Point.getMean(cluster));
	    }
	    return newCenters;
	}
	
	// 기존 중심점들과 새로운 중심점들간 거리의 합 반환
	public static double getDistance(List<Point> oldCenters, List<Point> newCenters) {
		double accumDist = 0;
		for (int i = 0; i < oldCenters.size(); i++) {
			double dist = oldCenters.get(i).getDistance(newCenters.get(i));
			accumDist += dist;
		}
		return accumDist;
	}
	
	// 새로운 중심점이 고정될때까지 반복 수행
	public static List<Point> kmeans(List<Point> centers, List<Point> dataset, int k) {
	    boolean converged;
	    do {
	    	// 새로운 중심점 목록
	        List<Point> newCenters = getNewCenters(dataset, centers);
	        // 기존 중심점들과 새로운 중심점의 거리들간 거리의 합
	        double dist = getDistance(centers, newCenters);
	        centers = newCenters;
	        converged = dist == 0;
	    } while (!converged);
	    return centers;
	}
	
	// 메인 메서드	
	public static void main(String[] args) {
		
		// 기존에 쌓인 데이터
		String inputFile = "C://java97/workspace/kmeans/src/kmeans/data/points2.txt";
		int k = 4;	    
	    
	    // 파일을 점목록(Ponit 클래스를 담는 리스트)으로 바꿔줌
	    List<Point> dataset = null;
		try {
		    dataset = getDataset(inputFile);
		} catch (Exception e) {
		    System.err.println("ERROR: Could not read file " + inputFile);
		    System.exit(-1);
		}
	    
		List<Point> centers = initializeRandomCenters(k, 0, 24);
		
		List<Point> kmeansCenterList = kmeans(centers, dataset, k); // 새로운 중심점 목록 결과
		
		// 새로운 데이터
		String inputFile2 = "C://java97/workspace/kmeans/src/kmeans/data/points3.txt";
		List<Point> dataset2 = null;
		try {
			dataset2 = getDataset(inputFile2);
		} catch (Exception e) {
			System.err.println("ERROR: Could not read file " + inputFile);
			System.exit(-1);
		}

		
		// 클러스터 범위 외 이벤트 발생시 true
		boolean exc = false;
		
		loof :
		for (Point center : kmeansCenterList) {
			// 점들 표시
			System.out.println("중심점: " + center);
			
			for (Point data : dataset2) {
				System.out.println("새로운 방문자가 들어온 시간" + data);
				// 범위 앞뒤로 30분
				exc = (data.getDistance(center) > 0.5);
				System.out.println(exc);
				if (exc == false) {
					System.out.println("정상 범위에 포함된 방문자");
					break loof;
				}
			}

		}
		
		if (exc) System.out.println("이상한 방문자");
		
		System.exit(0);
	}

}

```
